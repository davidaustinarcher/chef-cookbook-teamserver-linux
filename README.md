# Contrast-TeamServer Cookbook
This cookbook installs the Contrast TeamServer on Linux (Debian/Ubuntu or Redhat/Centos platforms) for demonstration purposes.  It is intended to automate the local installation and setup of TeamServer on a Linux VM hosted on a MacBook Pro with 16GB RAM.

*This cookbook was last tested with Contrast TeamServer v3.5.8.*

## Prerequisites
You will need to supply your own Contrast TeamServer installer file and license file.  More details about where to place your installer and license files are below.

### Add the Contrast TeamServer installer to the cookbook files
This cookbook does not include the Contrast TeamServer installer, which you can download using an account at https://hub.contrastsecurity.com.  The installer is not included with this cookbook because of its file size and it's best to get the latest version from hub.contrastsecurity.com.

1. Log in to your Contrast Hub account at https://hub.contrastsecurity.com
2. Go to the "Downloads" page and download your desired Linux installer
3. Once you've downloaded the TeamServer installer, which is a shell script with a filename of `Contrast-{version number}.sh`, move the installer file to the cookbook's `files/default` directory.  For example:
`mv ~/Downloads/Contrast-{version number}.sh ~/cookbook/location/contrast-teamserver/files/default/Contrast-{version number}.sh`
4. Next edit the default attributes file (`.../attributes/default.rb`) and update the `default['contrast-teamserver']['installer']` attribute value to match the TeamServer install filename (i.e., `Contrast-{version number}.sh`).

### Add your Contrast license to the cookbook files
A license file is purposely not included with this cookbook.  You will need to acquire your own license from your Contrast Account Team or Contrast Support.  After acquiring your license:
1. Copy/move the license file to the cookbook's `files/default` directory
2. Edit the `default.rb` attributes file (`.../attributes/default.rb`) and modify the value for `default['contrast-teamserver']['license']` to match the filename of your license file.

## Attributes
- `node['contrast-teamserver']['installer']` - Filename of the Contrast TeamServer installer
- `node['contrast-teamserver']['license']` - Filename of the Contrast license
- `node['contrast-teamserver']['teamserver_url']` - URL to access TeamServer, but please note the TeamServer web port is defined under the `vars.txt.erb` template under the `port_web$Long` setting
- `node['contrast-teamserver']['innodb_buffer_pool_size']` - TeamServer's MySQL `innodb_buffer_pool_size`

## Running this cookbook
From the `.../cookbooks` directory, run `sudo chef-client -zr 'recipe[contrast-teamserver]'`.

Please note that this cookbook will take some time to run (about a half hour on a MacBook Pro with 3.1 GHz Intel Core i7 and 16GB RAM) and TeamServer will still not be ready until about 15-30 minutes after the cookbook execution is complete due to the delay associated with installing, configuring, and initializing TeamServer for the first time.  Please allocate 45-60 minutes for TeamServer to be up, running, and accessible.

TeamServer will be fully running and accessible via your specified `teamserver_url` when you see a log message like `...Contrast TeamServer Ready - Took 1047250ms` from the `/opt/contrast/logs/server.log`.
