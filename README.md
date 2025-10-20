# iverilog_colab
---
Colab sessions reset when you close them or after some idle time.
👉 If you want to avoid reinstalling every time, clone and install iverilog into your Google Drive once.
```
from google.colab import drive
drive.mount('/content/drive')

# Step 1: Update package lists
!sudo apt-get update -y

# Step 2: Install all required build dependencies
!sudo apt-get install -y autoconf gperf make gcc g++ bison flex git build-essential automake libtool

# Step 3: Clone the repository and Build from source
%cd /content/drive/MyDrive
!git clone https://github.com/steveicarus/iverilog.git
%cd iverilog
!sh autoconf.sh
!./configure --prefix=/content/drive/MyDrive/iverilog-install
!make -j$(nproc)
!sudo make install

# Step 5: Verify
!iverilog -V

OR

%cd /content/drive/MyDrive
!git clone https://github.com/steveicarus/iverilog.git
%cd iverilog
!sh autoconf.sh
!./configure --prefix=/content/drive/MyDrive/iverilog-install
!make -j$(nproc)
!make install
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
