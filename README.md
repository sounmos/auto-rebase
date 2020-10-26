# power-git
正在开发的你是不是被很多个项目、很多个分支困扰，每次都需要切换到每个分支上，开始 `rebase、push`等一系列操作 ，然后在这痛并快乐的过程中持续同步 `master` 的最新代码。

现在好了。`power-git`可以一站式帮你解决这些问题。

#### 1. 如何开始？

首先我们需要全局安装我们的命令

```javascript
npm install team-auto-rebase-code -g
```

这样，在我们的电脑上会有这样一个命令

- `pgit`

如果你使用了 `nvm` 来管理你的 `node` 版本，一定要注意查看当前命令安装的目录是哪一个。也可以通过以下命令来查找

```shell
which pgit
```

做到这一步，本工具的安装就结束了，我们来看如何使用这个小工具。

#### 2. 如何使用

##### 2.1 使用之前，我们需要做这么几项配置

###### 1. 项目根目录设置（也就是我们的项目所在文件夹）

注意：是项目所在文件夹，不是项目文件夹

如：

```javascript
项目为 projct 
放置在 /Users/username/Desktop 下。那么 /Users/username/Desktop 就是我们的根目录
```

接下来我们通过下方命令来设置

```javascript
pgit -setroot /Users/username/Desktop
```

`setroot`后面跟的是一个绝对路径。不要设置相对路径，这样会导致项目找不到，从而不能正确使用本工具。

建议：

建议大家将多个项目放置在同一目录下。否则会出现配置比较繁琐的情况。

###### 2. 接下来我们需要设置rebase代码的配置文件。

首先创建一个 `pull-config.json` 文件

接下来我们根据下面的格式来书写我们的配置

```json
[
  {
    "project": "test", 
    "branch": [
      "branch1",
      "branch2",
      "branch3",
      "branch4",
    ]
  }
]
```

说明：

- 整个配置文件是由一个数组包裹。

- `project` 和 `branch` 为固定命名。

- `project` 代表项目名称。可以根据根目录写相对位置。

  - 如：

    > test 项目 位于 /Users/username/Desktop/one/two/three 项目下
    >
    > 那么这里的 project就可以配置为 “/one/two/three/test”

好。相信到这里，大家已经将配置文件创建成功了。接下来我们使用下面的命令关联配置文件

```javascript
pgit -config  /Users/username/Desktop/pull-config.json
```

注意：

`-config` 后跟的也是一个绝对路径。同 `-setroot`



#### 3. 开始同步

上面的两个配置完成后，我们就可以通过本工具进行代码同步。同步代码的命令就是 

```javascript
pgit 
```

注意：

如果同步有错误或有冲突的话，会将日志文件提示在桌面上。日志文件会精准的记录同步时间、项目名称、分支名称、错误信息等内容。



#### 4. 开启定时同步

除了可以手动同步我们的代码之外，本工具还支持定时同步。（注：由于兼容性问题，暂时只支持 Mac 下的定时同步工作）

开启定时同步的方法也比较简单，可以通过以下命令开启

```shell
pgit -auto
```

默认情况下，我们会每天执行一次同步任务。这里需要注意的一点是，如果你的电脑关机重启了，需要再次执行此命令，用来恢复定时任务。

##### 4.1 自动同步的时间设置

当你开启自动同步之后，就需要有一个特定的时间来同步代码。我们可以通过文件导入或在线配置的方式来实现时间的配置。**最小支持的时间跨度是每分钟执行一次。**

定时任务支持设置按照  `日、月、周、小时、分钟` 来设置执行时间。以下例子以 `每日` 来说明。配置方式适用于`日、月、周、小时、分钟` 所有内容

时间格式请按照 [自动同步](https://juejin.im/post/6884913497377898504) 中的 `crontab` 内容进行设置。

**支持范围：**

- 日：`1-31`
- 月：`1-12`
- 周：`0-6`  --  `0`代表周天
- 小时：`0-23`
- 分钟：`0-59`

###### 1. 在线配置

通过下面的命令进入在线配置定时的时间

```shell
pgit -time
```

输入命令之后，会提示你如何设定自己的时间。这里设置时间有以下几个格式

-  确定式：明确的知道需要哪个时间同步代码。

  - 如：清楚的知道`3`号的早上需要同步代码

    > 将 day 设置为 3 -->  day: 3

- 范围式：清楚的知道自己在哪个范围内需要同步代码

  - 例1：在每月的`1-10`号都需要每天同步代码

    > 将 day 设置为时间范围，将间隔时间 step 设置为 1
    >
    > day: 1-10
    >
    > step: 1

  - 例2：在每月的`1-10`号需要每隔两天同步一次代码

    > 将 day 设置为时间范围，将间隔时间 step 设置为 2
    >
    > day: 1-10
    >
    > step: 2

- 间隔式：知道哪几天需要同步，哪些时间不需要同步

  - 例子：每月的 `1,3,5,7,9` 号需要同步代码

    > 将day设置为 1,3,5,7,9

注意：step如果不设置则默认为1。

###### 2. 文件配置

可通过编写文件的方式来实现定时时间配置。文件格式如下

```json
[
  {
    type: 'week', // 周
    time: '',
    step: 1,
  },
  {
    type: 'month', // 月
    time: '',
    step: 1,
  },
  {
    type: 'day', // 日
    time: '',
    step: 1,
  },
  {
    type: 'hour', // 小时
    time: '',
    step: 1,
  },
  {
    type: 'minute', // 分钟
    time: '',
    step: 1,
  },
]
```

当然，文件配置也支持 `确定式、范围式、间隔式` 这几种设置方式 。

将文件编写好之后，可以通过

```shell
pgit -timefile 绝对路径
```

的方式来引入时间文件。



#### 5. 其他命令

`-v -V -version`：用来获取版本信息

`-h -help`: 用来获取帮助信息  