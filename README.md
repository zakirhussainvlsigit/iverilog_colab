# iverilog_colab
---
Colab sessions reset when you close them or after some idle time.
👉 If you want to avoid reinstalling every time, clone and install iverilog into your Google Drive once.

## STEP1:
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
## STEP2:
```
!mkdir -p /content/drive/MyDrive/verilog_projects
%cd /content/drive/MyDrive/verilog_projects
!mkdir fa
%cd fa

%%writefile fa.v
module fa(input a, b, cin, output sum, cout);
  assign {cout, sum} = a + b + cin;
endmodule

%%writefile tb_fa.v
`timescale 1ns/1ps
module tb_fa;
  reg a, b, cin;
  wire sum, cout;
  integer i;

  fa uut(.a(a), .b(b), .cin(cin), .sum(sum), .cout(cout));

  initial begin
  for(i=0;i<=7;i=i+1) begin
  {a,b,cin}=i;
  #10;
  $display($time,"a %b b %b cin %b cout %b sum %b",a,b,cin,cout,sum);
  end
  #10;
  $finish;
  end

  initial begin
    $dumpfile("fa.vcd");
    $dumpvars(1, tb_fa);
    end
endmodule
```
## STEP3:
```
!iverilog tb_fa.v fa.v
```

## STEP4:
```
!vvp a.out
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
