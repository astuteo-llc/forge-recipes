#!/bin/bash

# Astuteo Server Setup
#

# Exit immediately if a simple command exits with a non-zero status (https://ss64.com/bash/set.html)
set -e

# Helper functions
error_msg () { echo -e "\e[1;31m*** Error:\e[0m $1"; }
success_msg () { echo -e "\e[32m*** $1\e[0m"; }
info_msg () { echo -e "\e[34m*** $1\e[0m"; }
print_msg () { echo -e ">> $1"; }

# Install the nginx partials from https://github.com/nystudio107/nginx-craft
install_mysql_config () {

  if [ ! -d "/etc/mysql/conf.d/astuteo" ]; then
    print_msg "Installing mysql conf.ds"
    git clone git@github.com:astuteo-llc/forge-recipes.git astuteo-temp;
    cp -R astuteo-temp/mysql/conf.d /etc/mysql/conf.d;
    rm -rf astuteo-temp;
  fi

  success_msg "mysql conf.d added! Restart MySQL"
}


perform_craftcms_server_setup () {

  # Check if the user is root
  user="$(id -un 2>/dev/null || true)"
  if [ "$user" != 'root' ]; then

    error_msg 'this installer needs the ability to run commands as root.
We are unable to find either "sudo" or "su" available to make this happen.'
    exit 1
  fi

  success_msg "User is root :-)"

  # Install Steps
  install_mysql_config

  success_msg "Astuteo setup on Forge completed!"
}

info_msg "Start Astuteo setup on Forge"

perform_astuteo_server_setup
