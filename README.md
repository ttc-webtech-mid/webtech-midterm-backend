# Kanbong Kanban (Backend)
ระบบส่งการบ้าน   
Web technology and Web Services course's midterm project   
Academic year 2021   

นายปฏิภาณ บุญสิมมา 6210400710   
นายพงศ์พล โรจนอดิศร 6210401252   
นายกลวัชร เสถียรเสรีสุข 6210450016


# Installation

ใช้คำสั่ง `npm install` เพื่อติดตั้งแพคเกจที่จำเป็น ได้แก่   
 - strapi   
 - sqlite   

## Set up your database environment

เพื่อป้องกันข้อมูลส่วนบุคคล สร้างไฟล์ `.env` ที่ root directory ซึ่งมีข้อมูลดังต่อไปนี้   

 - VUE_APP_API_ENDPOINT="your server's listening port"   
 localhost:1337 will be the default   

## API ROUTES

หลังจากติดตั้งแพคเกจแล้ว ใช้คำสั่ง `npm run develop` เพื่อเริ่มต้นการใช้งาน server ของ strapi   

**/students**    

 - ดึงข้อมูลนักเรียนทั้งหมด: `GET /students`  
 
- ดึงข้อมูลนักเรียนด้วย ID ที่กำหนด: `GET /students/:id`    
`Params: id:id นักเรียนตามตารางในฐานข้อมูล`   

- ดึงข้อมูลด้วยรหัสนักเรีียน: `GET /students?std_id`    
`Query String: std_id=รหัสนิสิต`    

- ดึงข้อมูลนักเรียนที่ลงทะเบียนเรียนในวิชาที่กำหนด: `GET /students?courses.course_id`    
`Query String: course_id=รหัสวิชา`   