@@ -84,7 +84,7 @@ git clone https://github.com/gpakosz/.tmux.git ~/.tmux
ln -s -f ~/.tmux/.tmux.conf ~/
cp -v $RPATH/CONFIGS/tmux.conf.local ~/.tmux.conf.local

#nvim
# nvim
wget https://github.com/neovim/neovim/releases/download/stable/nvim-linux64.tar.gz -O /tmp/nvim-linux64.tar.gz
sudo tar xzvf /tmp/nvim-linux64.tar.gz --directory=/opt
sudo ln -s /opt/nvim-linux64/bin/nvim /usr/bin/nvim
@@ -97,6 +97,10 @@ sudo rm -f /opt/nvim-linux64.tar.gz
cat $RPATH/kitty-installer.sh | sh /dev/stdin
# ~/.local/kitty.app/bin/kitty

# batcat
wget https://github.com/sharkdp/bat/releases/download/v0.24.0/bat_0.24.0_amd64.deb -O /tmp/bat.deb
sudo dpkg -i /tmp/bat.deb

# Clone polybar & picom repos
mkdir ~/github
git clone --recursive https://github.com/polybar/polybar ~/github/polybar
git clone https://github.com/ibhagwan/picom.git ~/github/picom
# install polybar
cd ~/github/polybar
mkdir build
cd build
cmake ..
make -j$(nproc)
sudo make install
# install polybar themes
git clone --depth=1 https://github.com/adi1090x/polybar-themes.git ~/github/polybar-themes
chmod +x ~/github/polybar-themes/setup.sh
cd ~/github/polybar-themes
echo 1 | ./setup.sh
# install picom
cd ~/github/picom
git submodule update --init --recursive
meson --buildtype=release . build
ninja -C build
sudo ninja -C build install
# Change timezone
# To list timezones run: timedatectl list-timezones
sudo timedatectl set-timezone "Europe/Madrid"
mkdir ~/screenshots
# copy all config files
cp -rv $RPATH/CONFIGS/config/* ~/.config/
# copy scripts
cp -rv $RPATH/SCRIPTS/* ~/.config/polybar/forest/scripts/
sudo ln -s ~/.config/polybar/forest/scripts/target.sh /usr/bin/target
sudo ln -s ~/.config/polybar/forest/scripts/screenshot.sh /usr/bin/screenshot
# copy wallpapers
mkdir ~/Wallpapers/
cp -rv $RPATH/WALLPAPERS/* ~/Wallpapers/
# Set execution perms
chmod +x ~/.config/bspwm/bspwmrc
chmod +x ~/.config/bspwm/scripts/bspwm_resize
chmod +x ~/.config/polybar/launch.sh
chmod +x ~/.config/polybar/forest/scripts/target.sh
chmod +x ~/.config/polybar/forest/scripts/screenshot.sh
# Select rofi theme
#rofi-theme-selector
# Enable tap to click and change mousepad scroll direction (laptops) https://cravencode.com/post/essentials/enable-tap-to-click-in-i3wm/
#sudo mkdir -p /etc/X11/xorg.conf.d && sudo tee <<'EOF' /etc/X11/xorg.conf.d/90-touchpad.conf 1> /dev/null
#Section "InputClass"
#        Identifier "touchpad"
#        MatchIsTouchpad "on"
#        Driver "libinput"
#        Option "Tapping" "on"
#        Option "NaturalScrolling" "on"
#EndSection
#
#EOF
# Clean files
rm -rf ~/github
rm -rf $RPATH
sudo apt autoremove -y
echo -e "\n[+] Done. Have fun!\n"
echo -e "[!] Please reboot..\n"
