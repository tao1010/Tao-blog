---
title: 微信小程序-API-文件
date: 2018-05-31 09:48:02
tags: 微信小程序
categories: 小程序
---
	
1.wx.saveFile	 - 保存文件到本地	
	
	saveFile 会把临时文件移动，因此调用成功后传入的 tempFilePath 将不可用；
	本地文件存储大小限制为10M；
	object参数说明：
		tempFilePath 需要保存的文件的临时路径 必填 String
		success/fail/complete 回调函数	
	success返回参数说明：
		saveFilePath 文件保存的路径
	eg:
	wx.saveFile({
      tempFilePath: this.data.tempFilePath,
      success: function(res){
        that.setData({
          saveFilePath: res.savedFielPath
        })
        wx.setStorageSync('saveFilePath', res.savedFielPath)
        console.log(res)
      },
      fail: function(res){
        console.log(res)
      }
    })
2.wx.getFileInfo - 获取临时文件信息
	
	object参数说明：
		filePath 本地文件路径 必填 String
		digestAlgorithm 计算文件摘要的算法，默认值 md5，有效值：md5，sha1
		success/fail/complete 回调函数
	success参数说明：
		size 文件大小 B
		digest  按照传入的 digestAlgorithm 计算得出的的文件摘要
		errMsg 调用结果
	eg:
	//获取临时文件信息
	  getFile: function () {
	
	    wx.getSavedFileList({
	      success: function(res){
	
	        if(res.fileList.length > 0){
	
	          var tempFilePath = res.fileList[0].filePath
	          wx.getFileInfo({
	            filePath: tempFilePath,
	            success: function (res) {
	              console.log(res)
	            },
	            fail: function (res) {
	              console.log(res)
	            }
	          })
	        }
	      }
	    })
	  },
3.wx.getSavedFileInfo - 获取本地文件的文件信息(已保存的)
	
	object参数说明：
		filePath 本地文件路径 必填 String
		success/fail/complete 回调函数
	success参数说明：
		size 文件大小 B
		createTime 文件的保存时间的时间戳 从1970/01/01/ 08:00:00 到当前时间的秒数 Number
		errMsg 调用结果
	eg:
	getFileInfo: function(){

	    wx.getSavedFileList({
	      success: function (res) {
	
	        if (res.fileList.length > 0) {
	
	          var tempFilePath = res.fileList[0].filePath
	          wx.getSavedFileInfo({
	            filePath: tempFilePath,
	            success: function (res) {
	              console.log(res)
	            },
	            fail: function (res) {
	              console.log(res)
	            }
	          })
	        }
	      }
	    })
	  },
4.wx.getSavedFileList - 获取本地已保存的文件列表
    
    object参数说明：
    	success/fail/complete 回调函数	
    success参数说明：
	    errMsg 调用结果 String
	    fileList 文件列表 objectArray
	 fileList说明：
	 	filePath 文件本地路径 String
	 	createTime 文件的保存时间的时间戳 从1970/01/01/ 08:00:00 到当前时间的秒数 Number
	 	size 文件大小 kb Number
    eg:
    getList: function(){
	    wx.getSavedFileList({
	      success: function(res){
	        console.log(res)
	      }
	    })
    },
5.wx.removeSavedFile - 删除本地存储的文件
	
	object参数说明:
		filePath - 需要删除的文件路径 必填
		success/fail/complete 回调函数
    eg:
    removeFile: function(){

	    wx.getSavedFileList({
	      success: function (res) {
	
	        if (res.fileList.length > 0) {
	
	          var tempFilePath = res.fileList[0].filePath
	          wx.removeSavedFile({
	            filePath: tempFilePath,
	            success: function (res) {
	              console.log(res)
	            },
	            fail: function (res) {
	              console.log(res)
	            }
	          })
	        }
	      }
	    })
	  },
6.wx.openDocument - 新开页面打开文档
	
	支持格式:doc, xls, ppt, pdf, docx, xlsx, pptx
	object参数说明:
		filePath 文件路径，可通过 downFile 获得 必填 String
		fileType	文件类型，指定文件类型打开文件，有效值 doc, xls, ppt, pdf, docx, xlsx, pptx
	    success/fail/complete 回调函数
    eg:
    openDocument: function(){

	    wx.getSavedFileList({
	      success: function (res) {
	
	        if (res.fileList.length > 0) {
	
	          var tempFilePath = res.fileList[0].filePath
	          wx.openDocument({
	            filePath: tempFilePath,
	            fileType: 'pdf',
	            success: function (res) {
	              console.log(res)
	            },
	            fail: function (res) {
	              console.log(res)
	            }
	          })
	        }
	      }
	    })
	  },
参考资料：	
1.[文件](https://developers.weixin.qq.com/miniprogram/dev/api/file.html)		
