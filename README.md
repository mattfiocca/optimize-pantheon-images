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

There are two flavors of this script available. `optimize_pantheon_images` and `optimize_pantheon_images_sshpass`. 
The [sshpass](http://sourceforge.net/projects/sshpass/) flavor will prompt you once for your pantheon password, 
then brokers it to both rsync calls on your behalf.
This is used so that you aren't prompted mutiple times for your password, so that the script can just run. 

```
curl -O -L http://downloads.sourceforge.net/project/sshpass/sshpass/1.05/sshpass-1.05.tar.gz && tar xvzf sshpass-1.05.tar.gz
./configure
make
sudo make install
sshpass -V
```

## What this script is doing

1. First checks to see if you have jpegtran (and sshpass if using that flavor), aborts otherwise.

2. Prompts you for the absolute or relative path to your local /files directory. This directory can be anywhere actually, and you don’t have to be `cd`’d into the project or anything like that. This is just what you want the working directory to be on your local system. The script checks to make sure the path actually exists, aborts otherwise.

3. Prompts you for the pantheon site uuid. This is that long uuid found in your pantheon dash, i.e. XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX

4. If you are using sshpass, prompts for your pantheon password

5. The script then remembers where you were in your terminal and moves into your working directory, and starts downloading (via rsync) the entire /files directory of the remote site recursively. This probably goes without saying, but your local disk needs to have enough available space to support all your site files.

6. Then it builds a list of all .jpg and .jpeg images recursively and case-insensitively (.jpg or .JPG) through the entire working directory. 

7. Optimizes images. jpegtran offers two ways to optimize images; with either the `-optimize` flag or the `-progressive` flag. We’ve chosen `-progressive` in this script because its the method of optimization we prefer for web images, where images can be loaded almost instantly to the browser and then progressively download in sort of an async fashion. An additional flag we’re using in this script is the `-copy none` setting. This strips the image of all meta information, adding another layer of file size reduction.

8. After optimization, the images are then uploaded (again via rsync) back to the server recursively. 

9. When it’s all said and done, this script will `cd` you back to the directory that you came from before running the script.

## Bonus

You’ll notice that I’ve wrapped this functionality into a bash function, and then just call the 
function immediately after declaration. This is for fun things like sourcing into your ~/.bash_profile. 

For example, remove the function call at the end of this script and save the script to `~/optimize_pantheon_images.sh`. 
Then add this to end of `~/.bash_profile`:

```
. ~/optimize_pantheon_images.sh
```

Don’t forget the period at the beginning. Save and close, then start a new terminal session. Now, you can just call `optimize_pantheon_images`
anywhere on the terminal and it will return you back to where you were prior to the call.

## Future Plans

1. Add PNG optimizations