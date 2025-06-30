# MUST LEARN BEFORE XSS

🔹 Step 1: HTML ke Tags ka Role samajh

Tag	Kaam	Penetration Testing mein Kya Faida?

| Tag             | Kaam                      | Penetration Testing mein Kya Faida?                      |
| --------------- | ------------------------- | -------------------------------------------------------- |
| `<a>`           | Link banata hai           | `href=` mein `javascript:` inject ho sakta hai           |
| `<img>`         | Image show karta hai      | `onerror=` ka use karke XSS chalayenge                   |
| `<script>`      | JavaScript likhne ke liye | Direct XSS trigger karta hai                             |
| `<input>`       | User input field          | Reflected XSS test point hota hai                        |
| `<div>` / `<p>` | Content show karta hai    | Agar innerHTML use ho raha ho to vulnerable ho sakta hai |

---