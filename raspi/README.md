## config and tips for raspi

For ffmpeg see this https://squarepenguin.co.uk/forums/thread-964.html
wget https://github.com/ccrisan/motioneye/wiki/precompiled/ffmpeg_3.1.1-1_armhf.deb
sudo dpkg -i ffmpeg_3.1.1-1_armhf.deb

# Convert all addon stuff to https

grep -r --color=ALWAYS "http:" * | more
find . -iname "*.xml" -print0 | xargs -0 -I % perl -pi -e 's/http\:/https\:/g'  %
find . -iname "*py"   -print0 | xargs -0 -I % perl -pi -e 's/http\:/https\:/g'  %

