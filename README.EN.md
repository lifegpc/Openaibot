# Openaibot

OpenAI Chat Telegram Bot

DOCS Translate by deepl

[EN_README](https://github.com/sudoskys/Openaibot/blob/main/README.EN.md)

Writing with OpenAi on Telegram. Python > 3.7.

This program operates with `Api` authentication `Token` and is not an inverse of `chatGPT`, **Python implementation of chatGPT** implemented by the bot itself.

```
This is not an official OpenAI product. This is a personal project and is not affiliated with OpenAI in any way.
```

**Not responsible for any content generated by the bot, content provided by OpenAi**

**Implemented chatGPT myself, basically the same experience, except the Api costs money

*Homebrew dependency library, no Api request rate limit made

## Features

* chatGPT api version implementation, not reverse the preview api
* Support for private chats with senseless replies
* Support for rate limiting
* Support for whitelist system
* Support for blacklisting
* Support for single continuation
* Support for content filtering
* (20221205) Dependency library does not support asynchronous, large number of requests will block, replace with your own asynchronous library

See https://github.com/sudoskys/Openaibot/issues/1

## Initialization

* Pull/update process

The install script will automatically backup and restore the configuration, run it in the root directory (not in the program directory)
If it's a small update you can just ``git pull``.

```shell
curl -LO https://raw.githubusercontent.com/sudoskys/Openaibot/main/setup.sh && sh setup.sh
```

``cd Openaibot``

## Configure

### Configure Redis

```shell
## local
apt-get install redis

# Docker + persistence (saved in . /redis directory)
docker run --name redis -d -v $(pwd)/redis:/data -p 6379:6379 redis redis-server --save 60 1 --loglevel warning
```

### Configure dependencies

```bash
pip install -r requirements.txt
```

``pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple``

### Filters

Data/Danger.form One line of one blacklisted vocabulary. There must be at least one.

If not, the program will automatically pull down the cloud default list, and subsequent updetects will pull the cloud to overwrite the local one.

You can turn this filter off by placing a one-line list, but I don't favour you doing this.

### Config/app.toml

`cp app_exp.toml app.toml`

`vim app.toml`
`nano app.toml`

**configuration file**

``toml
[bot]
master = [100, 200] # master user id &owner
botToken = 'key'
OPENAI_API_KEY = ['key'] # multiple key loads
INTRO = "POWER BY OPENAI" # Suffix
ABOUT = "Created by github.com/sudoskys/Openaibot"

[proxy]
status = false
url = "http://127.0.0.1:7890"
```

[Telegram botToken request](https://t.me/BotFather)

[OPENAI_API_KEY request](https://beta.openai.com/account/api-keys), supports multiple key distribution load (automatic failover is not supported.)

## Run

* Run

```shell
nohup python3 main.py > /dev/null 2>&1 & 
```

* View the process

```shell
ps -aux|grep python3
```

* Terminate a process
  followed by the process number

```shell
kill -9  
```

## command

| command                   | role                        | extra                                  |
|---------------------------|-----------------------------|----------------------------------------|
| `/set_user_cold`          | set user_cooldown           | can't_send                             |
| `/set_group_cold`         | set_group_cooldown          | can't_send                             | 
| `/set_token_limit`        | Set the output limit length |                                        |
| `/set_user_cold`          | set_input_limit_length      |                                        |
| `/config`                 | get/backup config.json file | send file                              |
| `/add_block_group`        | disable                     | direct effect                          |
| `/del_block_group`        | unblock                     | take effect directly                   |
| `/add_block_user`         | disable                     | take effect directly                   |
| `/del_block_user`         | unblock                     | take effect directly                   |
| `/add_white_group`        | Add                         | Requires whitelist mode to take effect |
| `/add_white_user`         | Add                         | Requires whitelist mode to take effect |
| `/del_white_group`        | del_white_group             | Requires whitelist mode to be enabled  |
| `/del_white_user`         | Delist                      | Requires whitelist mode to take effect |
| `/update_detect`          | update_sensitive_words      |                                        |
| `/open_user_white_mode`   | Enable user whitelisting    |                                        |
| `/open_group_white_mode`  | open_group_white_listing    |                                        |
| `/close_user_white_mode`  | close_user_white_listing    |                                        |
| `/close_group_white_mode` | turn off group whitelisting |                                        |
| `/open`                   | Set user cooldown time      |                                        |
| `/close`                  | set user cooldown time      |                                        |

### Sample table

```markdown
set_user_cold - sets the user cooldown time
set_group_cold - sets the group cooldown time
set_token_limit - sets the output limit length
set_user_cold - sets the input limit length
config - get/backup config.json file
add_block_group - disable a group
del_block_group - unblock a group
add_block_user - disable users
del_block_user - unblock user
add_white_group - add a whitelisted group
add_white_user - add whitelisted users
del_white_group - delist a whitelisted group
del_white_user - delist a whitelisted person
update_detect - update sensitive words
open_user_white_mode - open user whitelist
open_group_white_mode - open group whitelist
close_user_white_mode - turn off user whitelisting
close_group_white_mode - turn off group whitelisting
open - sets the user cooldown time
close - sets the user cooldown time
```

## Other

### Statistics

``analysis.json`` is the frequency statistic, the number of requests within 60s.

### Config.json

will automatically merge missing keys for repair.

Quick Dev by MVC framework https://github.com/TelechaBot/BaseBot

