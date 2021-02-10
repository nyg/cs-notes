# Ghost blog

https://members.nearlyfreespeech.net/forums/viewtopic.php?t=10690

```sh
# update ghost-cli
cd /home/protected/ghost-cli
ghost --version
npm install ghost-cli

# update ghost blog
cd /home/protected/blog
ghost version
ghost update --force # --no-restart

# logs locations
cd /home/logs
cd /home/protected/blog/content/logs

# remove unused ghost blog versions
cd /home/protected/blog/versions
rm -rf <version-number>

# clean yarn cache
cd /home/private
yarn cache clean
```
