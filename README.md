# this script is based on http://linux-sunxi.org/Building_on_Debian and derivated from http://karme.de/olimex/olinuxino-a20-micro-image-builder to
# create a basic debian image for the olinuxino a20 micro
# (s.a. http://linux-sunxi.org/A20-OLinuXino)
#
# This script is intended to be used in a clean debian/wheezy virtual
# machine maybe cross-compiling (tested on amd64 using kvm) or to run
# a self-hosted native compile (tested using network block device via
# xnbd-client but didn't work for me yet - if it works for you please
# report it!)
#
# 1 - Download it 
# 2 - set it into /usr/local/bin
# sudo cp olinuxino-a20-micro-image-builder /usr/local/bin
# sudo chmod a+x  /usr/local/bin/olinuxino-a20-micro-image-builder
# 3 - create the user and the environement
# sudo olinuxino-a20-micro-image-builder setup
#
# Notes:
# - has to be run as root and is somewhat dangerous
# - you need around 15G free disk space
# - downloads code from github
# - at the moment uses emdebian for cross compilation tool chain (if cross-compiling)
# - script must be installed in path as it calls itself
# - root password is root !
# - consoles use auto-login (no password!)
# - ideally the boards would be supported in mainline kernel
#   s.a. http://linux-sunxi.org/Linux_mainlining_effort
# - ideally one would add support for the board (and similar boards)
#   to the debian installer and/or freedom-maker / freedombox
# - maybe you want to take a look at more general Board Support
#   Package (BSP) at http://linux-sunxi.org/BSP


# Copyright (c) 2013 Jens Thiele <karme@karme.de>
# 
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
# 
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
#
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in the
#    documentation and/or other materials provided with the distribution.
#
# 3. Neither the name of the authors nor the names of its contributors
#    may be used to endorse or promote products derived from this
#    software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
# A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
# OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED
# TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
# PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
# LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
# NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.