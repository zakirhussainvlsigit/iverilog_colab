# iverilog_colab
---
Colab sessions reset when you close them or after some idle time.
👉 If you want to avoid reinstalling every time, clone and install iverilog into your Google Drive once.
```
from google.colab import drive
drive.mount('/content/drive')

# Install required dependencies
!sudo apt-get update -y
!sudo apt-get install -y autoconf gperf make gcc g++ bison flex git build-essential automake libtool

# Clone iverilog
%cd /content/drive/MyDrive
!git clone https://github.com/steveicarus/iverilog.git
%cd iverilog

# Build
!sh autoconf.sh
!./configure --prefix=/content/drive/MyDrive/iverilog-install
!make -j$(nproc)

# Fix permission for mkinstalldirs
!chmod +x mkinstalldirs

# Install (no sudo here)
!make install

# Add to PATH for this session
import os
os.environ['PATH'] += ":/content/drive/MyDrive/iverilog-install/bin"

# Verify
!iverilog -V


```

---
```
And in future sessions, just add it to your PATH:

import os
os.environ['PATH'] += ":/content/drive/MyDrive/iverilog-install/bin"
!iverilog -V

from google.colab import drive
import os

drive.mount('/content/drive')
```
---
```
# Restore execute permission
!chmod +x /content/drive/MyDrive/iverilog-install/bin/iverilog
!chmod +x /content/drive/MyDrive/iverilog-install/bin/vvp

# Add to PATH
os.environ['PATH'] += ":/content/drive/MyDrive/iverilog-install/bin"

# Test
!iverilog -V
```
