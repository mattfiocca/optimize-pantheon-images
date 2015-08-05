# Optimize Pantheon Images
Simple bash script to optimize images on pantheon systems

## Dependancies

### jpegtran

This script relies on `jpegtran` to be installed and present in the `$PATH` of your local system. 
jpegtran is a command found in the [libjpeg](http://libjpeg.sourceforge.net/) library, 
which comes bundled with [ImageMagick](http://www.imagemagick.org/). 
If you’ve installed imagemagick for some other reason, 
you’ve already got jpegtran installed. If not, it’s just a matter of: 

**Ubuntu/Debian**
```
sudo apt-get install gcc
sudo apt-get install imagemagick
```

**CentOS/Fedora/RHEL**
```
yum install gcc php-devel php-pear
yum install ImageMagick ImageMagick-devel
```

**Mac** (using homebrew)
```
brew install imagemagick
```

### sshpass

This script will prompt you once for your pantheon password, then [sshpass](http://sourceforge.net/projects/sshpass/) 
will broker your password to both rsync calls on your behalf.
This is used so that you aren't prompted mutiple times for your password, so that the script can just run. 

```
curl -O -L http://downloads.sourceforge.net/project/sshpass/sshpass/1.05/sshpass-1.05.tar.gz && tar xvzf sshpass-1.05.tar.gz
./configure
make
sudo make install
sshpass -V
```

If you do not wish to install sshpass, you can just omit the following from the beginning of the rsync calls:

```
sshpass -p "$PASSWORD"
```

## Bonus

You’ll notice that I’ve wrapped this functionality into a bash function, and then just call the 
function immediately after declaration. This is for fun things like sourcing into your ~/.bash_profile. 

For example, remove the function call at the end of this script and save the script to `~/optimize_pantheon_jpgs.sh`. 
Then add this to end of `~/.bash_profile`:

```
. ~/optimize_pantheon_jpgs.sh
```

Don’t forget the period at the beginning. Save and close, then start a new terminal session. Now, you can call optimize_pantheon_jpgs 
directly on the terminal from anywhere and it will return you back to where you were terminal’ed into previous to the call.