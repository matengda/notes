```bash
#!/bin/bash

cp_call() {
    adb pull /sdcard/MIUI/sound_recorder/call_rec ./call
} # 通话录音

cp_camera() {
    adb pull /sdcard/DCIM/Camera ./camera
} # 照相机

cp_screenshots() {
    adb pull /sdcard/DCIM/Screenshots ./screenshots
} # 截屏


rm_call() {
    adb shell rm -f /sdcard/MIUI/sound_recorder/call_rec/*
}

rm_camera() {
    adb shell rm -f /sdcard/DCIM/Camera/*
}

rm_screenshots() {
    adb shell rm -f /sdcard/DCIM/Screenshots/*
    adb shell rm -f /sdcard/DCIM/Screenshots/.*
}

rm_thumbnails() {
    adb shell rm -f /sdcard/DCIM/.thumbnails/*
    adb shell rm -f /sdcard/DCIM/.thumbnails/.*
} # 缩略图


# begin_wechat
id=

cp_id_voice2() {
    adb pull /sdcard/tencent/MicroMsg/${id}/voice2
} # 语音

cp_weixin() {
    adb pull /sdcard/tencent/MicroMsg/WeiXin ./weixin
} # 附件

mv_id_voice2() {
    if test ! -d ./voice; then
        mkdir ./voice
    fi

    for i in `find ./voice2 -name "*.amr"`; do
        mv -v $i ./voice
    done
}

rm_id_attachment() {
    adb shell rm -rf /sdcard/tencent/MicroMsg/${id}/attachment/*
} # 附件

rm_id_avatar() {
    adb shell rm -rf /sdcard/tencent/MicroMsg/${id}/avatar/*
} # 头像

rm_id_brandicon() {
    adb shell rm -f /sdcard/tencent/MicroMsg/${id}/brandicon/*
} # 公众号图标

rm_id_image() {
    adb shell rm -rf /sdcard/tencent/MicroMsg/${id}/image/*
} # 图片

rm_id_image2() {
    adb shell rm -rf /sdcard/tencent/MicroMsg/${id}/image2/*
} # 这里是微信聊天图片

rm_id_music() {
    adb shell rm -f /sdcard/tencent/MicroMsg/${id}/music/*
} # 音乐

rm_id_sns() {
    adb shell rm -rf /sdcard/tencent/MicroMsg/${id}/sns/*
} # 朋友圈

rm_id_video() {
    adb shell rm -f /sdcard/tencent/MicroMsg/${id}/video/*
} # 视频

rm_id_voice2() {
    adb shell rm -rf /sdcard/tencent/MicroMsg/${id}/voice2/*
} # 语音

rm_weixin() {
    adb shell rm -f /sdcard/tencent/MicroMsg/WeiXin/*
} # 附件下载目录
# end_wechat


# begin_kingofglory
cp_king_record() {
    adb pull /sdcard/tencent/shouyoubao/ManualScreenRecord ./record
} # 王者荣耀录屏

rm_king_record() {
    adb shell rm -f /sdcard/tencent/shouyoubao/ManualScreenRecord/*
}

rm_king_img() {
    adb shell rm -f /sdcard/tencent/shouyoubao/ScreenRcordImg/*
}
# end_kingofglory


cp_one() {
    cp_screenshots
    cp_weixin
}

cp_auto() {
    cp_camera
    cp_screenshots
    cp_call
    cp_id_voice2
    mv_id_voice2
    cp_weixin
    cp_king_record
}

rm_one() {
    rm_screenshots
    rm_weixin
}

rm_auto() {
    rm_camera
    rm_screenshots
    rm_thumbnails
    rm_call
    rm_id_attachment
    rm_id_avatar
    rm_id_brandicon
    rm_id_image
    rm_id_image2
    rm_id_music
    rm_id_sns
    rm_id_video
    rm_id_voice2
    rm_weixin
}

rm_lh() {
    for i in *; do
        if test $i = ${0:2}; then
            continue
        fi

        rm -rv $i
    done
}

$1
```





关闭广告

miui -> 个人账号 -> 隐私协议等 -> 系统广告





miui备份文件位置

/sdcard/MIUI/Backup/AllBackup







红米9rom下载

国内

https://bigota.d.miui.com/V11.0.3.0.QJCCNXM/lancelot_images_V11.0.3.0.QJCCNXM_20200701.0000.00_10.0_cn_fd290402bd.tgz

欧洲

https://bigota.d.miui.com/V11.0.3.0.QJCEUXM/lancelot_eea_global_images_V11.0.3.0.QJCEUXM_20200614.0000.00_10.0_eea_ac7965aae7.tgz



解锁bootloader

unlock.update.miui.com
此连接是解锁工具官网

http://miuirom.xiaomi.com/rom/u1106245679/4.5.813.51/miflash_unlock-4.5.813.51.zip
此链接是小米解锁工具下载地址

miui -> 设置 -> 我的设备 -> 全部参数 -> MIUI 版本
此操作进入开发者选项, 如果点击 内核版本 5次进入cit

miui -> 设置 -> 更多设置 -> 开发者选项 -> 设备解锁状态 -> 绑定账号和设备
此操作 必须插入SIM卡, 必须使用手机移动网络, 必须绑定账号后等待168个小时