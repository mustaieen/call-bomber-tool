#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Call Bomber Tool - Premium Neon Design
Developer: Mustaieen
"""

import os
import sys
import time
import requests
import unicodedata

# ── Neon Colors ─────────────────────────────
R  = '\033[91m'   # Neon Red
G  = '\033[92m'   # Neon Green
Y  = '\033[93m'   # Neon Yellow
B  = '\033[94m'   # Neon Blue
M  = '\033[95m'   # Neon Magenta
C  = '\033[96m'   # Neon Cyan
W  = '\033[97m'   # White
BOLD = '\033[1m'  # Bold
RS = '\033[0m'    # Reset

API_URL = "https://api-modifyxhub.tech/custom.php?api_action=live-call&number="
DELAY = 1.0  # Fixed delay (seconds)

# Box width constant (characters inside the box)
BOX_WIDTH = 64

# ═══════════════════════════════════════════
# STRING WIDTH HELPER (For perfect border alignment)
# ═══════════════════════════════════════════
def get_display_width(text):
    """Calculate the actual display width of a string (handles emojis/wide chars)"""
    width = 0
    for char in text:
        if unicodedata.east_asian_width(char) in ('W', 'F'):
            width += 2
        else:
            width += 1
    return width

def print_box_line(color, content, left_padding=1, right_padding=1):
    """Prints a perfectly aligned line inside a box"""
    # Calculate visible length without ANSI color codes
    import re
    ansi_escape = re.compile(r'\x1B(?:[@-Z\\-_]|\[[0-?]*[ -/]*[@-~])')
    visible_text = ansi_escape.sub('', content)
    
    current_width = get_display_width(visible_text)
    total_padding_needed = BOX_WIDTH - current_width
    spaces = " " * max(0, total_padding_needed - left_padding - right_padding)
    
    print(f"{color}║{RS}{' ' * left_padding}{content}{spaces}{' ' * right_padding}{color}║{RS}")

# ═══════════════════════════════════════════
# PREMIUM BANNER & FONT
# ═══════════════════════════════════════════
def banner():
    os.system('clear' if os.name != 'nt' else 'cls')
    
    # ── Big Call Bomber Font ──
    call_bomber = f"""{R}{BOLD}
   ██████╗ █████╗ ██╗     ██╗        ██████╗  ██████╗ ███╗   ███╗██████╗ ███████╗██████╗ 
  ██╔════╝██╔══██╗██║     ██║        ██╔══██╗██╔═══██╗████╗ ████║██╔══██╗██╔════╝██╔══██╗
  ██║     ███████║██║     ██║        ██████╔╝██║   ██║██╔████╔██║██████╔╝█████╗  ██████╔╝
  ██║     ██╔══██║██║     ██║        ██╔══██╗██║   ██║██║╚██╔╝██║██╔══██╗██╔══╝  ██╔══██╗
  ╚██████╗██║  ██║███████╗███████╗   ██████╔╝╚██████╔╝██║ ╚═╝ ██║██████╔╝███████╗██║  ██║
   ╚═════╝╚═╝  ╚═╝╚══════╝╚══════╝   ╚═════╝  ╚═════╝ ╚═╝     ╚═╝╚═════╝ ╚══════╝╚═╝  ╚═╝
    {RS}"""
    print(call_bomber)
    
    # ── Developer Info Box ──
    print(f"{M}{BOLD}  ╔{'═' * BOX_WIDTH}╗{RS}")
    print_box_line(M+BOLD, f"{Y}{BOLD}Developer : {W}Mustaieen {Y}⚡")
    print_box_line(M+BOLD, f"{C}{BOLD}Status    : {G}Online")
    print(f"{M}{BOLD}  ╚{'═' * BOX_WIDTH}╝{RS}\n")


# ═══════════════════════════════════════════
# MENU DESIGN
# ═══════════════════════════════════════════
def menu():
    print(f"{B}{BOLD}  ╔{'═' * BOX_WIDTH}╗{RS}")
    print_box_line(B+BOLD, f"{C}{BOLD}[1]{RS} {W}Start Call Bombing")
    print_box_line(B+BOLD, f"{R}{BOLD}[2]{RS} {W}Exit Tool")
    print(f"{B}{BOLD}  ╚{'═' * BOX_WIDTH}╝{RS}\n")


# ═══════════════════════════════════════════
# SINGLE CALL FUNCTION
# ═══════════════════════════════════════════
def send_call(number):
    headers = {
        "User-Agent": "Mozilla/5.0 (Linux; Android 10) AppleWebKit/537.36 "
                      "(KHTML, like Gecko) Chrome/120.0 Mobile Safari/537.36",
        "Accept": "application/json, text/plain, */*",
    }
    try:
        r = requests.get(API_URL + str(number), headers=headers, timeout=15)
        return r.status_code, r.text[:120]
    except requests.exceptions.Timeout:
        return None, "Timeout"
    except requests.exceptions.ConnectionError:
        return None, "Connection Error"
    except Exception as e:
        return None, str(e)


# ═══════════════════════════════════════════
# BOMBING FUNCTION
# ═══════════════════════════════════════════
def bombing():
    # --- Target Number Input ---
    while True:
        number = input(f"  {C}{BOLD}[?] Target Number : {RS}").strip()
        if number:
            break
        print(f"  {R}[!] Number cannot be empty! Try again.{RS}")

    # --- Total Calls Input ---
    while True:
        try:
            count_input = input(f"  {C}{BOLD}[?] How many calls : {RS}").strip()
            count = int(count_input) if count_input else 40
            if count <= 0:
                print(f"  {R}[!] Calls must be greater than 0!{RS}")
                continue
            break
        except ValueError:
            print(f"  {R}[!] Invalid input! Enter a number.{RS}")

    print()
    print(f"{G}{BOLD}  ╔{'═' * BOX_WIDTH}╗{RS}")
    print_box_line(G+BOLD, f"{W}TARGET NUMBER : {R}{BOLD}{number}")
    print_box_line(G+BOLD, f"{W}TOTAL CALLS   : {R}{BOLD}{count}")
    print(f"{G}{BOLD}  ╚{'═' * BOX_WIDTH}╝{RS}\n")

    print(f"{G}{BOLD}  ╔{'═' * BOX_WIDTH}╗{RS}")
    print_box_line(G+BOLD, f"{W}STATUS        : {R}{BOLD}BOMBING STARTED...")
    print(f"{G}{BOLD}  ╚{'═' * BOX_WIDTH}╝{RS}\n")

    ok, fail = 0, 0
    
    try:
        for i in range(1, count + 1):
            status, body = send_call(number)
            stamp = time.strftime("%H:%M:%S")

            if status and 200 <= status < 300:
                ok += 1
                print(f"  {C}┣━{RS} {M}[{stamp}]{RS} {G}Call {i:02d}/{count} [OK] Success{RS}")
            else:
                fail += 1
                print(f"  {C}┣━{RS} {M}[{stamp}]{RS} {R}Call {i:02d}/{count} [X] Failed{RS}")

            time.sleep(DELAY)
            
    except KeyboardInterrupt:
        print(f"\n\n  {R}[!] Bombing stopped by user.{RS}")

    # --- Result Summary ---
    print(f"\n{Y}{BOLD}  ╔{'═' * BOX_WIDTH}╗{RS}")
    print_box_line(Y+BOLD, f"{W}FINAL REPORT")
    print(f"{Y}{BOLD}  ╠{'═' * BOX_WIDTH}╣{RS}")
    print_box_line(Y+BOLD, f"{G}[OK] Success : {ok}")
    print_box_line(Y+BOLD, f"{R}[X] Failed   : {fail}")
    print(f"{Y}{BOLD}  ╚{'═' * BOX_WIDTH}╝{RS}\n")
    
    input(f"  {C}Press Enter to return to menu...{RS}")


# ═══════════════════════════════════════════
# MAIN LOOP
# ═══════════════════════════════════════════
def main():
    while True:
        banner()
        menu()
        choice = input(f"  {C}{BOLD}└─> Select Option : {RS}").strip()

        if choice == "1":
            bombing()
        elif choice == "2":
            print(f"\n  {G}{BOLD}Thank you for using this tool!{RS}")
            print(f"  {W}{BOLD}  Developer : {M}Mustaieen {Y}\n")
            sys.exit(0)
        else:
            print(f"\n  {R}[!] Invalid option! Please select 1 or 2.{RS}")
            time.sleep(1.5)


if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print(f"\n\n  {R}Exiting...{RS}\n")
        sys.exit(0)
