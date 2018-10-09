## 广告对象初始化
使用如下方式初始化退出广告

    mUpExitAd = UPExitAd.getInstance(this);

## 显示退出广告
在用户希望退出游戏时，调用`show()`方法显示退出广告

**请注意在显示之前需要调用`isReady()`方法判断当前是否有广告可以显示**

    if (mUpExitAd != null && mUpExitAd.isReady()) {
        // 此处应该先对游戏进行暂停处理

        mUpExitAd.show();
    } else {
        // 没有退出广告可以显示，直接退出游戏

        finish();
    }

## 回调接口
退出广告可以通过`setUpExitAdListener`方法设置回调接口，原则上您只需要关心`onExit`和`onCancel`两个回调方法，来判断用户是否确认退出游戏

    mUpExitAd.setUpExitAdListener(new UPExitAdListener() {
        @Override
        public void onClicked() {
            // 此处为广告点击的回调
        }

        @Override
        public void onClickedMore() {
            // 此处为更多按钮点击的回调
        }

        @Override
        public void onDisplayed() {
            // 此处为广告展示的回调
        }

        @Override
        public void onExit() {
            // 此处为用户确认退出的回调

            finish();
        }

        @Override
        public void onCancel() {
            // 此处为用户点击取消的回调

            // 此处应该对游戏进行恢复
        }

    });