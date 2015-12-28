<!--
author: zhengyu
date: 2015-12-28 13:25:02
title: yii2使用验证码组件
tags: yii2
category: php
status: publish
summary: yii2使用验证码组件
-->

yii2自带的例子里面有一个关于验证码的使用例子，但是本篇文章讨论的是用另外一种方式手动生成和验证验证码。

####生成验证码

在要使用验证码的Controller里面实现actions方法：

```php
class TestController extends Controller{
	public function actions(){
		return [
            'captchatest' => [
                'class' => 'yii\captcha\CaptchaAction',
                'maxLength' => 4, //生成的验证码最大长度
                'minLength' => 4  //生成的验证码最短长度
            ]
        ];
	}	
}
```

以上代码通过实现actions方法创建了一个叫captchatest的action，上面的action我只填了两个参数，还有其他参数可以参考```yii\captcha\CaptchaAction```的publish属性

####在页面中使用验证码

在要使用验证码的view里面插入以下代码：

```php
<?php echo Captcha::widget(['name'=>'captchaimg','captchaAction'=>'captchatest','imageOptions'=>['id'=>'captchaimg', 'title'=>'换一个', 'alt'=>'换一个', 'style'=>'cursor:pointer;margin-top:10px; height: 22px;'],'template'=>'{image}']); ?>
```

以上代码主要需要正确填写captchaAction，填写你刚才创建的captchaAction，需要完整的namespace，然后会生成一个img

####验证验证码

在action中接收到表单传来的验证码后，使用：

```php
$this->createAction('captchatest')->validate($captchCode, false); //$captchCode为用户输入的验证码
```

validate函数会返回true/false，该函数的第二个参数为是否对大小写敏感

####刷新验证码

生成的验证码有时用户看不清楚，需要重新刷新，可以使用该图片的url加上refresh参数，然后会返回一个json数据，其中有一个url的属性，调用该url即可获取新验证码，如图片地址为：```/index.php?r=test%2Fcaptchatest&v=5680ce41e9cb0```，获取图片地址为：```/index.php?r=feedback%2Fcaptchafeedback&v=5680ce41e9cb0&refresh=1```