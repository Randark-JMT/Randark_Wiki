---
title: 流量分析
description: 
published: true
date: 2022-07-11T01:33:43.175Z
tags: 
editor: markdown
dateCreated: 2022-07-11T01:33:43.174Z
---

# 流量分析
## 键盘流量分析
```python
normalKeys = {"04": "a", "05": "b", "06": "c", "07": "d", "08": "e", "09": "f", "0a": "g", "0b": "h", "0c": "i", "0d": "j", "0e": "k", "0f": "l", "10": "m", "11": "n", "12": "o", "13": "p", "14": "q", "15": "r", "16": "s", "17": "t", "18": "u", "19": "v", "1a": "w", "1b": "x", "1c": "y", "1d": "z", "1e": "1", "1f": "2", "20": "3", "21": "4", "22": "5", "23": "6", "24": "7", "25": "8", "26": "9", "27": "0",
              "28": "<RET>", "29": "<ESC>", "2a": "<DEL>", "2b": "\t", "2c": "<SPACE>", "2d": "-", "2e": "=", "2f": "[", "30": "]", "31": "\\", "32": "<NON>", "33": ";", "34": "'", "35": "<GA>", "36": ",", "37": ".", "38": "/", "39": "<CAP>", "3a": "<F1>", "3b": "<F2>", "3c": "<F3>", "3d": "<F4>", "3e": "<F5>", "3f": "<F6>", "40": "<F7>", "41": "<F8>", "42": "<F9>", "43": "<F10>", "44": "<F11>", "45": "<F12>"}
shiftKeys = {"04": "A", "05": "B", "06": "C", "07": "D", "08": "E", "09": "F", "0a": "G", "0b": "H", "0c": "I", "0d": "J", "0e": "K", "0f": "L", "10": "M", "11": "N", "12": "O", "13": "P", "14": "Q", "15": "R", "16": "S", "17": "T", "18": "U", "19": "V", "1a": "W", "1b": "X", "1c": "Y", "1d": "Z", "1e": "!", "1f": "@", "20": "#", "21": "$", "22": "%", "23": "^", "24": "&", "25": "*",
             "26": "(", "27": ")", "28": "<RET>", "29": "<ESC>", "2a": "<DEL>", "2b": "\t", "2c": "<SPACE>", "2d": "_", "2e": "+", "2f": "{", "30": "}", "31": "|", "32": "<NON>", "33": "\"", "34": ":", "35": "<GA>", "36": "<", "37": ">", "38": "?", "39": "<CAP>", "3a": "<F1>", "3b": "<F2>", "3c": "<F3>", "3d": "<F4>", "3e": "<F5>", "3f": "<F6>", "40": "<F7>", "41": "<F8>", "42": "<F9>", "43": "<F10>", "44": "<F11>", "45": "<F12>"}
output = []
# tshark -r keyboard.pcapng -T fields -e usbhid.data | sed '/^\s*$/d' > usbdata.txt
keys = open('usbdata.txt')
for line in keys:
    import re
    b = re.findall(r'.{2}', line)
    line = '-'.join(b)
    try:
        if line[0] != '0' or (line[1] != '0' and line[1] != '2') or line[3] != '0' or line[4] != '0' or line[9] != '0' or line[10] != '0' or line[12] != '0' or line[13] != '0' or line[15] != '0' or line[16] != '0' or line[18] != '0' or line[19] != '0' or line[21] != '0' or line[22] != '0' or line[6:8] == "00":
            continue
        if line[6:8] in normalKeys.keys():
            output += [[normalKeys[line[6:8]]],
                       [shiftKeys[line[6:8]]]][line[1] == '2']
        else:
            output += ['[unknown]']
    except:
        pass
keys.close()

flag = 0
print("".join(output))
for i in range(len(output)):
    try:
        a = output.index('<DEL>')
        del output[a]
        del output[a-1]
    except:
        pass
for i in range(len(output)):
    try:
        if output[i] == "<CAP>":
            flag += 1
            output.pop(i)
            if flag == 2:
                flag = 0
        if flag != 0:
            output[i] = output[i].upper()
    except:
        pass
print('output :' + "".join(output))
```
## 工控流量分析
### MODBUS
```python
# 分析操作码
import pyshark 
def get_code():
     captures = pyshark.FileCapture("modbus1.pcap") #设置Modbus流量包路径
     func_codes = {}
     for c in captures:
         for pkt in c:
             if pkt.layer_name == "modbus":
                 func_code = int(pkt.func_code)
                 if func_code in func_codes:
                     func_codes[func_code] += 1
                 else:
                     func_codes[func_code] = 1
     print(func_codes)
if __name__ == '__main__':
 get_code()
 ```
 ```python
 # 针对操作码进行数据筛选
import pyshark
def find_flag():
     cap = pyshark.FileCapture("modbus1.pcap") #设置Modbus流量包路径
     idx = 1
     for c in cap:
         for pkt in c:
             if pkt.layer_name == "modbus":
                 func_code = int(pkt.func_code)
                 if func_code == 16: #查看对应功能码寄存器下写入的值
                     payload = str(c["TCP"].payload).replace(":", "")
                     print(hex_to_ascii(payload))
                     print("{0} *".format(idx))
         idx += 1
def hex_to_ascii(payload):
 data = payload
 flags = []
 for d in data:
     _ord = ord(d)
     if (_ord > 0) and (_ord < 128):
         flags.append(chr(_ord))
 return ''.join(flags)
if __name__ == '__main__':
 find_flag()
 ```