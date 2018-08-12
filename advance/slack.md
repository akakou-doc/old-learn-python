# SlackBotを作ろう

## Slackとは
ビジネス用のコミュニケーションツールです。

## SlackBotとは
Slack用のBotです。
応用すれば以下に使えます。
* 好きなサイトの更新を通知する
* 不審なログを通知する

## 環境構築
### ワークスペース
Slack Botを作るためには、Slackのチームが必要です。  
詳しくは[こちら](https://get.slack.help/hc/ja/articles/206845317-Slack-%E3%83%AF%E3%83%BC%E3%82%AF%E3%82%B9%E3%83%9A%E3%83%BC%E3%82%B9%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B)を見て下さい

### Bots
ワークスペースにBotsを導入する必要があります。  
[こちらから](https://tnct-dorm-net.slack.com/apps/A0F7YS25R-bots)インストールして下さい。  
またそのAPI Tokenは扱いに注意して、メモをして下さい。

### [slack-clients](https://github.com/slackapi/python-slackclient)
pipを使ってインストールしましょう。
```bat
pip install slackclient
```

## Let's Hack
基本的に詳しくは[こちら]ということでお願いします。

### メッセージの送信

```py
from slackclient import SlackClient

slack_token = 'BotsのAPIキーをここに貼る'
sc = SlackClient(slack_token)

sc.api_call(
    "chat.postMessage",
    channel="#チャンネル名",
    text="メッセージ
)

```

### メッセージの受け取り
#### とりあえず受け取る
```py
import time
from slackclient import SlackClient

slack_token = 'BotsのAPIキーをここに貼る'
sc = SlackClient(slack_token)

if sc.rtm_connect():
    while sc.server.connected is True:
        # ここで読み込んでる
        print(sc.rtm_read())
        time.sleep(1)
else:
    print("Connection Failed")
```

辞書型の空のメッセージとか、長ったらしいメッセージが流れてくる。

#### メッセージだけを抽出する
```py
import time
from slackclient import SlackClient

slack_token = 'BotsのAPIキーをここに貼る'
sc = SlackClient(slack_token)


if sc.rtm_connect():
    while sc.server.connected is True:
        # メッセージを取得
        message_list = sc.rtm_read()
        time.sleep(1)
        
        if not message_list:
            # 取得したメッセージが
            # 空だったら無視
            continue

        for message in message_list:
            if message["type"] != 'message':
                # メッセージのタイプが
                # messageじゃなかったら無視
                continue
        
            print(message["text"])
else:
    print("Connection Failed")
```

メッセージのテキストだけ表示されるようになった。


### pythonってきたら、「いいぞぉ〜」って返す

```py
import time
from slackclient import SlackClient


class IizoBot:
    '''
    「python」ってきたら「いいぞぉ〜」っていう
    こだまでしょうか？いいえIizoBot。
    '''
    def __init__(self, token):
        '''
        Slack Clientの初期化
        '''
        self.sc = SlackClient(token)

    def send(self, channel):
        '''
        Slackに「いいぞぉ」って投げる
        '''
        self.sc.api_call(
            "chat.postMessage",
            channel=channel,
            text="いいぞぉ〜"
        )

    def analyze(self, message_list):
        '''
        メッセージのリストを受け取り
        その中から、返信すべきメッセージだけ抽出する
        '''
        if not message_list:
            # 取得したメッセージが
            # 空だったら無視
            return []

        result = []
        for message in message_list:
            if message["type"] != 'message':
                # メッセージのタイプが
                # messageでなかったら無視
                continue
        
            if message["text"] != 'python':
                # メッセージの内容が
                # python出なかったら無視
                continue

            result.append(message)

        return result

    def run(self):
        '''実行'''
        if not self.sc.rtm_connect():
            # コネクションがはれなかったら無視
            print("Connection Failed")
            return

        while self.sc.server.connected is True:
            # メッセージを取得
            message_list = self.sc.rtm_read()
            time.sleep(1)

            for message in self.analyze(message_list):
                self.send(message['channel'])


if __name__ == "__main__":
    slack_token = 'BotsのAPIキーをここに貼る'
    iizo_bot = IizoBot(slack_token)
    iizo_bot.run()
```


