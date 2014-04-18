##  先学会composer相关的基本知识吧
-------------
工欲善其事必先利其器  包管理工具是基本知识 不懂这个还真有点麻烦 下面介绍composer的基本知识点 内容来自一篇blog介绍：
[composer-primer](http://daylerees.com/composer-primer)

php包管理器的引入可谓是一个革命性的壮举！也令好多phper感动的落泪（这么夸张！）
composer就是用来管理包依赖关系的工具，在其他流行语言中包依赖都有管理工具的 比如：
  * npm
  * maven
  * gem
  ......

## composer.json

假设我们要开发一个项目，且不论是project（完整的项目） 还是library（类库）我们只认为其是一个包(package)而已
这个包想要被composer管理，除了初始化的目录外 唯一要做的就是再添加一个composer.json 的文件（在项目根目录下） 。
这个文件就是项目针对composer的配置文件。它使用了js语言的JSON格式来描述各种配置信息。

先赖创建一个简单的composer文件吧：
~~~
{
    // 包名称 反斜杠分隔的两部分 前面一般是github账号项目主人 后面是项目名
    // 虽然可以是其他东西但最好遵从惯例写法
    "name":             "yiiSpace/year",
    // 包描述 尽量简洁
    //  许多开发者使用寄宿在github上的仓库名称 如果包准备开源 那么详细描述可放在README中
    "description":      "Yii2  Extension and Application Repository for YiiSpace project",
    // 关键字 搜索时会用到 也要简明扼要
    "keywords":         ["php","yii", "extension", "yiispace"],
    // 主页 可以是项目的主页 或者是github的仓库地址
    "homepage":         "https://github.com/yiiSpace/year",
    "type":             "yii2-extension",
    // 项目初次发布的日期 格式如： YYYY-MM-DD 或者 YYYY-MM-DD HH:MM:SS.
    "time":             "2014-4-19",
    // 选择一个合适的协议 许多协议要求在源码根目录下存一个协议拷贝 但如果你同时提供了这个声明
    // 那么项目可以根据协议被搜索到
    "license":          "MIT",
    "authors": [
        {
            "name":         "yiqing",
            "email":        "yiqing_95@qq.com",
            "homepage":     "https://github.com/yiqing-95",
            "role":         "project lead"
        }
    ]
}
~~~
其实上面的东西都是可选的！！ 但如果你确实想发布你的东西给世界其他角落的人使用 最好写上上面的信息
可在composer的官网查看全部的配置信息 [the composer website ](http://getcomposer.org/)
我们往往没那么多时间细看 所以最好有速成法 或者其他人告诉我们咋做就行 正如我现在写的这个东西。

上面的信息只是用来描述包的 下面开始依赖管理相关的东西：

## 依赖管理
~~~
"require": {
        // 统配符表示最近的版本
		"yiisoft/yii2": "*"
		// "ext-curl": "*"
	},
~~~
声明本项目依赖了哪些其他的项目，当然你可以拷贝源码到我们的目录下(vendor?  ) 但升级需要我们自己维护 当添加到require段
后 composer就会帮我们来自动维护版本问题。许多开源程序都寄宿在github或者bitbucket上 对应的版本控制系统也具有tagging的功能
如 为我们的项目打tag
~~~
  git tag -a 1.0.0 -m 'First version'
~~~
上面的命令让我们创建了一个1.0.0的版本，其他人可以依赖此稳定的发布版了
依赖最小或者最大的版本：
~~~
"xmen/gambit": ">1.0.0"

"xmen/gambit": "<1.0.0"

"xmen/gambit": "=>1.0.0"
"xmen/gambit": "=<1.0.0"

// 版本区间  1.0.1会被选中 如果有这个版本的话！
"xmen/gambit": ">1.0.0,<1.0.2"

// 依赖某个开发分支
"xmen/gambit": "dev-branchname"
~~~
我们依赖的包仍旧可能依赖其他包...这些包可能也有composer.php 的require段配置
指定最小的稳定依赖
> "minimum-stability": "dev"

只在开发时引入：
> "require-dev": {
     "codeception/codeception": "1.6.0.3"
  }

使用composer的 --dev选项


 // 根名称空间映射到的目录（以当前composer所在的目录为参照）
 "year\\": "src/"

-------------------------
可以参考这里[this handy cheat sheet ](http://composer.json.jolicode.com/) 我将内容拷贝过来
但缺少提示信息 你可以前往看看 鼠标放在某行上后 会有对应的解释 很详尽

可用的命令行：
~~~
$ php composer.phar install
$ php composer.phar update
$ php composer.phar require vendor-name/package-name
$ php composer.phar init
$ php composer.phar create-project symfony/framework-standard-edition dir/
$ php composer.phar run-script
$ php composer.phar search my keywords
$ php composer.phar validate
$ php composer.phar config --list
$ php composer.phar self-update
$ php composer.phar status
$ php composer.phar diagnose
$ php composer.phar help
$ php composer.phar show
$ php composer.phar global
$ php composer.phar licenses
$ php composer.phar archive
$ php composer.phar depends vendor-name/package-name
$ php composer.phar dump-autoload --optimize
~~~
这个是比较全的样例文件
~~~
{
    "name": "vendor-name/project-name",
    "description": "This is a very cool package!",
    "version": "0.3.0",
    "type": "library",
    "keywords": ["logging", "cool", "awesome"],
    "homepage": "http://jolicode.com",
    "time": "2012-12-21",
    "license": "MIT",
    "authors": [
        {
            "name": "Xavier Lacot",
            "email": "xlacot@jolicode.com",
            "homepage": "http://www.lacot.org",
            "role": "Developer"
        },
        {
            "name": "Benjamin Clay",
            "email": "bclay@jolicode.com",
            "homepage": "http://ternel.net",
            "role": "Developer"
        }
    ],
    "support": {
        "email": "support@exemple.org",
        "issues": "https://github.com/jolicode/GouvCamp-mobile/issues",
        "forum": "http://www.my-forum.com/",
        "wiki": "http://www.my-wiki.com/",
        "irc": "irc://irc.freenode.org/composer",
        "source": "https://github.com/jolicode/GouvCamp-mobile"
    },
    "require": {
        "monolog/monolog": "1.0.*",
        "joli/ternel": "@dev",
        "acme/foo": "dev-master#2eb0c0978d290a1c45346a1955188929cb4e5db7"
    },
    "require-dev": {
        "debug/dev-only": "1.0.*"
    },
    "conflict": {
        "another-vendor/conflict": "1.0.*"
    },
    "replace": {
        "debug/dev-only": "1.0.*"
    },
    "provide": {
        "debug/dev-only": "1.0.*"
    },
    "suggest": {
        "monolog/monolog": "Allows more advanced logging of the application flow"
    },
    "autoload": {
        "psr-4": {
            "Monolog\\": "src/",
            "Vendor\\Namespace\\": ""
        },
        "psr-0": {
            "Monolog": "src/",
            "Vendor\\Namespace": ["src/", "lib/"],
            "Pear_Style": "src/",
            "": "src/"
        },
        "classmap": ["src/", "lib/", "Something.php"],
        "files": ["src/MyLibrary/functions.php"]
    },
    "autoload-dev": {
        "psr-0": {
            "MyPackage\\Tests": "test/"
        }
    },
    "target-dir": "Symfony/Component/Yaml",
    "minimum-stability": "stable",
    "repositories": [
        {
            "type": "composer",
            "url": "http://packages.example.com"
        },
        {
            "type": "vcs",
            "url": "https://github.com/Seldaek/monolog"
        },
        {
            "type": "pear",
            "url": "http://pear2.php.net"
        },
        {
            "type": "package",
            "package": {
                "name": "smarty/smarty",
                "version": "3.1.7",
                "dist": {
                    "url": "http://www.smarty.net/files/Smarty-3.1.7.zip",
                    "type": "zip"
                },
                "source": {
                    "url": "http://smarty-php.googlecode.com/svn/",
                    "type": "svn",
                    "reference": "tags/Smarty_3_1_7/distribution/"
                }
            }
        }
    ],
    "config": {
        "process-timeout": 300,
        "use-include-path": false,
        "preferred-install": "auto",
        "github-protocols": ["git", "https", "http"],
        "github-oauth": {"github.com": "oauthtoken"},
        "vendor-dir": "vendor",
        "bin-dir": "bin",
        "cache-dir": "$home/cache",
        "cache-files-dir": "$cache-dir/files",
        "cache-repo-dir": "$cache-dir/repo",
        "cache-vcs-dir": "$cache-dir/vcs",
        "cache-files-ttl": 15552000,
        "cache-files-maxsize": "300MiB",
        "notify-on-install": true,
        "discard-changes": false
    },
    "archive": {
        "exclude": ["/foo/bar", "baz", "/*.test", "!/foo/bar/baz"]
    },
    "prefer-stable": true,
    "scripts": {
        "post-update-cmd": "MyVendor\\MyClass::postUpdate",
        "post-package-install": [
            "MyVendor\\MyClass::postPackageInstall"
        ],
        "post-install-cmd": [
            "MyVendor\\MyClass::warmCache",
            "phpunit -c app/"
        ]
    },
    "extra": { "key": "value" },
    "bin": ["./bin/toto"]
}
~~~

---------------------






