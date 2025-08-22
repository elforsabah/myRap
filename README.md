import pyautogui
import time

time.sleep(5)  # Time to focus the input after running (adjust if needed)
pyautogui.typewrite('1234567890')  # Replace with your test Vbeln value
pyautogui.press('enter')  # Simulates pressing Enter to trigger onNextStep
