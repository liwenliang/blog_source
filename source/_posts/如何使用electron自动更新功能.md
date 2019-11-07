---
title: 如何使用electron自动更新功能
tags:
  - electron
categories:
  - Web开发
toc: false
date: 2019-03-22 18:13:34
---

最近做了个项目，要用到electron的自动更新功能。

直接贴代码：

update.js

```javascript
// 检测更新，在你想要检查更新的时候执行，renderer事件触发后的操作自行编写
import { autoUpdater } from 'electron-updater'
import { ipcMain } from 'electron'

autoUpdater.autoDownload = false
const uploadUrl = 'http://xxx.xxx.xxx/soft/download/' // 下载地址，不加后面的**.exe

export function updateHandle(mainWindow) {
  const message = {
    error: '检查更新出错，请稍后再试！',
    checking: '正在检查更新……',
    updateAva: '检测到新版本',
    updateNotAva: '现在使用的是最新版本，无需更新！'
  }

  // 通过main进程发送事件给renderer进程，提示更新信息
  const sendUpdateMessage = (text) => {
    mainWindow.webContents.send('message', text)
  }

  autoUpdater.setFeedURL(uploadUrl)

  autoUpdater.on('error', () => {
    sendUpdateMessage(message.error)
  })

  autoUpdater.on('checking-for-update', () => {
    sendUpdateMessage(message.checking)
  })

  autoUpdater.on('update-available', (info) => {
    sendUpdateMessage(message.updateAva, info)
    mainWindow.webContents.send('update-available', info)
  })

  autoUpdater.on('update-not-available', (info) => {
    sendUpdateMessage(message.updateNotAva)
  })

  // 更新下载进度事件
  autoUpdater.on('download-progress', (downloadObj) => {
    mainWindow.webContents.send('downloadProgress', downloadObj)
  })

  autoUpdater.on('update-downloaded', (event, releaseNotes) => {
    mainWindow.webContents.send('update-downloaded', event, releaseNotes)
  })

  ipcMain.on('checkForUpdate', () => {
    // 执行自动更新检查
    autoUpdater.checkForUpdates()
  })

  ipcMain.on('downloadUpdate', () => {
    // 下载更新
    autoUpdater.downloadUpdate()
  })

  ipcMain.on('quitAndInstall', () => {
    // 退出并更新
    autoUpdater.quitAndInstall()
  })
}

```

update.vue

```html
<template>
  <div>
    <div style="text-align:center;">
      <el-button style="height: 5em;" :loading="loading" type="primary" icon="el-icon-refresh" @click="checkoutUpdate">
        {{ btnText }}
      </el-button>
      <p style="margin-top: 1em;">
        {{ tip }}
      </p>
    </div>

    <el-dialog :title="`已下载新版本${newVersion}`" :visible.sync="dialogVisible" width="30%">
      <p v-for="(item, idx) in features" :key="idx" class="item">
        {{ idx+1 }}.{{ item }}
      </p>
      <span slot="footer" class="dialog-footer">
        <el-button @click="onCancel">稍后更新</el-button>
        <el-button type="primary" @click="updateNow">立即更新</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { ipcRenderer } from 'electron'

export default {
  name: 'SettingUpdate',
  data() {
    return {
      tip: '',
      btnText: '检查最新版本！',
      dialogVisible: false,
      features: [],
      newVersion: '',
      loading: false
    }
  },
  created() {
    ipcRenderer.on('message', (event, text) => {
      this.tip = text
    })
    ipcRenderer.on('downloadProgress', (event, downloadObj) => {
      this.loading = true
      const downloadPercent = downloadObj.percent.toFixed(2)
      this.btnText = `正在下载(${downloadPercent}%)`
    })

    ipcRenderer.on('update-available', (event, info) => {
      this.$confirm(`检测到新版本${info.version}，是否下载更新?`, '新版本提示', {
        confirmButtonText: '下载',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        ipcRenderer.send('downloadUpdate')
      }).catch(() => {
      })
    })

    ipcRenderer.on('update-downloaded', (event, releaseNotes) => {
      console.log('--------releaseNotes--------')
      console.log(releaseNotes)
      console.log('--------releaseNotes--------')
      this.btnText - '检查最新版本！'
      this.loading = false
      this.dialogVisible = true
      this.tip = ''
      this.newVersion = releaseNotes.version
      this.features = releaseNotes.features
    })
  },
  methods: {
    checkoutUpdate() {
      ipcRenderer.send('checkForUpdate')
    },
    onCancel() {
      this.dialogVisible = false
    },
    updateNow() {
      this.dialogVisible = false
      ipcRenderer.send('quitAndInstall')
    }
  }
}
</script>

<style scoped lang="scss">
  .item {
    line-height: 1.5em;
    font-size: 1.2em;
  }
</style>


```

