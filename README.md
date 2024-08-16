# 1. senaryo : Vmess over Websocket (server-client)

```
{
"log": {
    "loglevel": "error"
  },
  "inbounds": [
    {
      "port": 80,
      "listen": "0.0.0.0",
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "6be3e1b2-05e1-46a1-ad36-70aaabaa8d12"
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "headers": {
            "Host": "www.google.com"
          },
          "path": "/vmess-ws"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "UseIP"
      }
    },
    {
      "protocol": "blackhole",
      "tag": "blackhole"
    }
  ],
  "routing": {
    "rules": [
      {
        "type": "field",
        "outboundTag": "blackhole",
        "ip": [
          "127.0.0.0/8"
        ]
      }
    ]
  }
}
```

```
{
  "log": {
    "loglevel": "info"
  },
  "inbounds": [
    {
      "port": 1080,
      "listen": "127.0.0.1",
      "protocol": "socks",
      "settings": {
        "auth": "noauth",
        "udp": true
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "ipadresim",
            "port": 80,
            "users": [
              {
                "id": "6be3e1b2-05e1-46a1-ad36-70aaabaa8d12",
                "security": "auto"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "headers": {
            "Host": "www.google.com"
          },
          "path": "/vmess-ws"
        }
      }
    }
  ]
}
```

# 2. senaryo : Vmess over Websocket over TLS (server-client)

```
{                                                                                          "log": {                                                                                   "loglevel": "info"                                                                     },                                                                                       "inbounds": [                                                                              {                                                                                          "port": 443,                                                                            "listen": "0.0.0.0",                                                                     "protocol": "vmess",                                                                     "settings": {                                                                              "clients": [                                                                               {                                                                                          "id": "6be3e1b2-05e1-46a1-ad36-70aaabaa8d12",                                            "alterId": 64,                                                                           "security": "zero"                                                                     }                                                                                      ]                                                                                      },                                                                                       "streamSettings": {                                                                        "network": "ws",                                                                         "security": "tls",                                                                       "tlsSettings": {                                                                           "certificates": [                                                                          {                                                                                          "certificateFile": "/root/certificate.crt",                                              "keyFile": "/root/private.key"                                                         }                                                                                      ],                                                                                       "allowInsecure": true,  // Allow insecure connections                                    "serverName": "api.whatsapp.net"                                                       },                                                                                       "wsSettings": {                                                                            "path": "/vmess-ws-public",                                                              "headers": {
            "Host": "api.whatsapp.net"                                                             }                                                                                      }                                                                                      }                                                                                      }                                                                                      ],                                                                                       "outbounds": [                                                                             {                                                                                          "protocol": "freedom",                                                                   "settings": {}                                                                         }                                                                                      ]                                                                                      }
```
