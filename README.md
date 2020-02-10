
# Aria2 RPC #

```sh
aria2c --enable-rpc=true --input-file=~/.aria2/aria2.session --conf-path=~/.aria2/aria2.conf
```

# 更新tracker #

[2](https://newtrackon.com/api/stable?include_ipv6_only_trackers=0) [3](https://newtrackon.com/api/stable)

```sh
curl -fs -C - \
  https://raw.githubusercontent.com/ngosang/trackerslist/master/{trackers_all,trackers_all_ws,trackers_all_ip}.txt \
  https://newtrackon.com/api/stable?include_ipv6_only_trackers=0 \
  | /usr/local/opt/gawk/bin/gawk NF \
  | /usr/local/opt/gnu-sed/bin/gsed ':a;N;s/\n/,/g;ta' \
  | /usr/local/opt/findutils/bin/gxargs -r -n 1 -I '{}' \
  /usr/local/opt/gnu-sed/bin/gsed -i '/^bt-tracker\=/{h;s#\(^bt-tracker\=\).*$#\1{}#};${x;/^$/{s##bt-tracker\={}#;H};x}' \
  /usr/local/etc/aria2/aria2.conf
```

