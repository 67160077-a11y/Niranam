การเข้าสู่เซิร์ฟเวอร์ (SSH)

เชื่อมต่อเซิร์ฟเวอร์ด้วยกุญแจส่วนตัว:ssh -i key.pem azureuser@niranam.duckdns.org

การตั้งค่า Geographic Firewall (บล็อกต่างประเทศ)

สร้างกลุ่มไอพีสำหรับประเทศไทย:sudo ipset create thailand hash:net

ดาวน์โหลดและเพิ่มรายชื่อไอพีเข้ากลุ่ม:curl -s [https://www.ipdeny.com/ipblocks/data/countries/th.zone](https://www.ipdeny.com/ipblocks/data/countries/th.zone) | while read line; do sudo ipset add thailand $line; done

สั่งบล็อกการเข้าถึงพอร์ต 80 และ 443 จากไอพีที่ไม่อยู่ในกลุ่ม:sudo iptables -I INPUT -p tcp -m multiport --dports 80,443 -m set ! --match-set thailand src -j DROP

ตรวจสอบสถานะกฎของ Firewall:sudo iptables -L INPUT -n --line-numbers

การตรวจสอบระบบป้องกันการโจมตี (Fail2ban)

ดูสถานะการทำงานและรายชื่อไอพีที่ถูกบล็อกจากการเดารหัสผ่าน:sudo fail2ban-client status sshd

การจัดการ Nginx และ Performance Tuning

ตรวจสอบความถูกต้องของไฟล์คอนฟิก Nginx ก่อนใช้งานจริง:sudo nginx -t

ตรวจสอบ HTTP Header เพื่อดูการทำงานของแคช:curl -I [https://niranam.duckdns.org/wp-includes/css/style.css](https://niranam.duckdns.org/wp-includes/css/style.css)

การสร้าง Dashboard วิเคราะห์ทราฟฟิก (GoAccess)

รันโปรแกรมเพื่อแปลง Log ของ Nginx เป็นหน้าเว็บ HTML (ธีมสีเข้ม):goaccess /var/log/nginx/access.log -o /var/www/html/dashboard.html --color-scheme=Monokai --real-time-html