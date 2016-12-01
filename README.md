#Read 

*看到这篇文章之后就把这个转过来了：

程序猿们可以说是这世界上最聪明的一群人了，但是人往往越聪明就越爱偷懒。程序猿们到底有多懒呢？通过GitHub（世界上最大的开源代码库）上的一个项目，我们可以看到其冰山一角。

这个项目里有着数个脚本文件，它们的名字都很奇葩，比如“smack-my-bitch-up.sh”(译：提醒我女人一下)，或者“kumar-asshole.sh”(译：Kumar是个SB)。该项目的管理员是一名叫Narkoz的用户，但是他表示，这些脚本是由他的一名前同事写的。Narkoz在项目介绍中写道：“我们的构建工程师跳槽了，这家伙简直生活在终端里。如果任何事情需要超过90秒的时间，他就会写个脚本去做这件事。这就是他所留下来的遗产”。

在这个人离职之后，他的前同事们开始整理他的文件和项目。他们发现，为了偷懒，这个人为他生活里无数个事情编写了脚本，比如他工作上的一些职责，他的感情生活，甚至冲咖啡。这些脚本都会把这些事情自动化，为他节省出大量的时间。

比如“smack-my-bitch-up.sh”，这个脚本的作用是：如果在晚上9点之后公司服务器里还有他账号的SSH连接，就自动给他老婆发送一条“今天要加班”的短信。而且，这个脚本还会从一系列预设好的原因里自动挑一个写入短信。

还有“kumar-asshole.sh”，从名字上来看，他显然很讨厌这名叫Kumar的客户。这个脚本会自动搜索他邮箱里来自这名客户的邮件，并且会注意查找“帮助”，“问题”，以及“对不起”等关键字。如果邮件里含有这些关键字，该脚本会自动把这名客户的数据库还原至最近的备份，并且对邮件进行自动回复：“别担心，下次注意就好”。

另外一个叫“hangover.sh”（译：宿醉）的脚本会在他早上8:45还没有登录服务器的时候自动给他的老板发一封邮件，给出一个借口，比如“今天不舒服，要在家里工作”。

但是，所有脚本里面最厉害的还要属于他的咖啡脚本。在运行这个脚本之后它会等待17秒，之后侵入办公室里的咖啡机开始制作拿铁。在准备好拿铁之后，这个脚本会让咖啡机等待24秒之后再把拿铁倒出来。而这一切费时加起来正好是从这个人的办公桌走到咖啡机所需要的时间。利害得失，除了他之外，没有人发现咖啡机有联网，并且可以被入侵修改。






# Hacker Scripts

Based on a _[true
story](https://www.jitbit.com/alexblog/249-now-thats-what-i-call-a-hacker/)_:

> xxx: OK, so, our build engineer has left for another company. The dude was literally living inside the terminal. You know, that type of a guy who loves Vim, creates diagrams in Dot and writes wiki-posts in Markdown... If something - anything - requires more than 90 seconds of his time, he writes a script to automate that.

> xxx: So we're sitting here, looking through his, uhm, "legacy"

> xxx: You're gonna love this

> xxx: [`smack-my-bitch-up.sh`](https://github.com/NARKOZ/hacker-scripts/blob/master/smack-my-bitch-up.sh) - sends a text message "late at work" to his wife (apparently). Automatically picks reasons from an array of strings, randomly. Runs inside a cron-job. The job fires if there are active SSH-sessions on the server after 9pm with his login.

> xxx: [`kumar-asshole.sh`](https://github.com/NARKOZ/hacker-scripts/blob/master/kumar-asshole.sh) - scans the inbox for emails from "Kumar" (a DBA at our clients). Looks for keywords like "help", "trouble", "sorry" etc. If keywords are found - the script SSHes into the clients server and rolls back the staging database to the latest backup. Then sends a reply "no worries mate, be careful next time".

> xxx: [`hangover.sh`](https://github.com/NARKOZ/hacker-scripts/blob/master/hangover.sh) - another cron-job that is set to specific dates. Sends automated emails like "not feeling well/gonna work from home" etc. Adds a random "reason" from another predefined array of strings. Fires if there are no interactive sessions on the server at 8:45am.

> xxx: (and the oscar goes to) [`fucking-coffee.sh`](https://github.com/NARKOZ/hacker-scripts/blob/master/fucking-coffee.sh) - this one waits exactly 17 seconds (!), then opens a telnet session to our coffee-machine (we had no frikin idea the coffee machine is on the network, runs linux and has a TCP socket up and running) and sends something like `sys brew`. Turns out this thing starts brewing a mid-sized half-caf latte and waits another 24 (!) seconds before pouring it into a cup. The timing is exactly how long it takes to walk to the machine from the dudes desk.

> xxx: holy sh*t I'm keeping those

Original: http://bash.im/quote/436725 (in Russian)  
Pull requests with other implementations (Python, Perl, Shell, etc) are welcome.

## Usage

You need these environment variables:

```sh
# used in `smack-my-bitch-up` and `hangover` scripts
TWILIO_ACCOUNT_SID=ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_AUTH_TOKEN=yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy

# used in `kumar_asshole` script
GMAIL_USERNAME=admin@example.org
GMAIL_PASSWORD=password
```

For Ruby scripts you need to install gems:
`gem install dotenv twilio-ruby gmail`

## Cron jobs

```sh
# Runs `smack-my-bitch-up.sh` monday to friday at 9:20 pm.
20 21 * * 1-5 /path/to/scripts/smack-my-bitch-up.sh >> /path/to/smack-my-bitch-up.log 2>&1

# Runs `hangover.sh` monday to friday at 8:45 am.
45 8 * * 1-5 /path/to/scripts/hangover.sh >> /path/to/hangover.log 2>&1

# Runs `kumar-asshole.sh` every 10 minutes.
*/10 * * * * /path/to/scripts/kumar-asshole.sh

# Runs `fucking-coffee.sh` hourly from 9am to 6pm on weekdays.
0 9-18 * * 1-5 /path/to/scripts/fucking-coffee.sh
```

---
Code is released under WTFPL.
