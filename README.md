# Kanbong Kanban (Strapi Headless CMS)
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

## Set up your environment

เพื่อป้องกันข้อมูลส่วนบุคคล สร้างไฟล์ `.env` ที่ root directory ซึ่งมีข้อมูลดังต่อไปนี้   

 - VUE_APP_API_ENDPOINT="your server's listening port"   
 localhost:1337 will be the default   

## API ROUTES

หลังจากติดตั้งแพคเกจแล้ว ใช้คำสั่ง `npm run develop` เพื่อเริ่มต้นการใช้งาน server ของ Strapi   

**/students**

 - ดึงข้อมูลนักเรียนทั้งหมด: `GET /students`  
 
- ดึงข้อมูลนักเรียนด้วย ID ที่กำหนด: `GET /students/:id`     
`Params: id:id นักเรียนตามตารางในฐานข้อมูล`   

- ดึงข้อมูลด้วยรหัสนักเรีียน: `GET /students?std_id`     
`Query String: std_id=รหัสนิสิต`     

- ดึงข้อมูลนักเรียนที่ลงทะเบียนเรียนในวิชาที่กำหนด: `GET /students?courses.course_id`       
`Query String: course_id=รหัสวิชา`     
- สำหรับการอัพเดตข้อมูลนักเรียน เช่น การอัพเดตคะแนน การ Assign งานให้นักเรียน สามารถใช้ได้ทั้ง 2 url:      
`PUT /students/:id`      
`PUT /students?std_id`     
`Body Request: integer:id หรือ Query String: std_id=รหัสนักเรียน`      

**/courses**
- ดึงข้อมูลวิชาเรียนทั้งหมด: `GET /courses`    

- ดึงข้อมูลวิชาเรียนด้วยรหัสวิชา: `GET /courses?course_id`     
`Query String: course_id=รหัสวิชา`    

- ดึงข้อมูลวิชาเรียนตามอาจารย์ผู้สอน: `GET /courses?teachers.id`     
`Query String: teachers.id=รหัสประจำตัวอาจารย์`       

- ดึงข้อมูลวิชาเรียนที่มีนักเรียนตามรหัสนักเรียนที่กำหนด: `GET /courses?students.std_id`    
`Query String: students.std_id=รหัสนักเรียน`    

**การ Assign งานของอาจารย์ผู้สอน**     
`API ENDPOINT POST /assignments`      

    headers: {
	    Authorization: `Bearer ${JWT}`
	}
	
    Body Request: {
	    topic: หัวข้องาน
	    deatil: รายละเอียดของงาน
	    due_date: วันครบกำหนดส่งงาน
	    course: วิชาเรียน
	    teacher: อาจารย์ผู้ให้งาน
	    students: Array [รหัสนักเรียนที่ลงทะเบียนเรียนในวิชานี้ทั้งหมดเป็น running number]
    }
**การส่งงานของนักเรียน**     
`API ENDPOINT POST /hand-in-assignments`    

    headers: {
	    Authorization: `Bearer ${JWT}`
	}
	
    Body Request : {
	    student: ไอดีของนักเรียนซึ่งเป็น running number ที่ทาง Strapi สร้างให้,
	    assignment: ไอดีของ Assignment เป็น running number
    } 

เนื่องจากการส่งงานของนักเรียนจำเป็นที่จะต้องอัพโหลดไฟล์ขึ้นที่ Server ของ Strapi ทำได้ดังนี้       
- การอัพโหลดไฟล์งาน: `POST /upload`    

    Strapi requires FormData to be sent to the server and link your file to given collection     
    ต้องส่ง Key ตามที่กำหนดไปใน FormData    
	 - files : ไฟล์ที่ต้องการ    
	 - ref: ตารางที่มี record ที่จะใช้เชื่อมกับไฟล์     
	 - field: คอลัมน์ที่เก็บไฟล์ในตารางจาก ref     
	 - refId: record ที่จะใช้เชื่อมกับไฟล์ที่อัพโหลด    
	***ข้อแนะนำ* *ควรสร้าง record ในตาราง hand-in-assignments ก่อนเพื่อที่จะได้ refId ของ record ที่จะเชื่อมกับไฟล์ที่จะอัพโหลด***       

**/rewards**      
- การแลกของรางวัล: `POST /rewards/:id`      

        headers: {
	        Authorization: `Bearer ${JWT}`
	    }
        Body Request : {
	        id: ไอดีของ reward เป็น running number,
	        students: Array[รหัสนักเรียนที่เป็น running number],
	        stocks: จำนวน reward คงเหลือ
        }
 - การเพิ่มของรางวัล (เฉพาะผู้ดูแลระบบเท่านั้น): `POST /rewards`      
 เช่นเดียวกันกับการ Assign การบ้าน การเพิ่มของรางวัลต้องมีการ upload รูปภาพ ซึ่งสามารถทำได้ในวิธีเดียวกัน     
 
        headers: {
	        Authorization: `Bearer ${JWT}`
	    }
        Body Request : {
	        reward_name:  ชื่อของ reward,
	        detail: รายละเอียดเกี่ยวกับ reward,
	        redeem_points:  คะแนนที่ต้องใช้ในการแลก,
	        stocks: จำนวนของ reward ที่มีทั้งหมด
        }
 - การแก้ไขของรางวัล (เฉพาะผู้ดูแลระบบเท่านั้น): `PUT /rewards/:id`      
 
        headers: {
	        Authorization: `Bearer ${JWT}`
	    }
        Body Request : {
	        reward_name:  ชื่อของ reward ,
	        detail: รายละเอียดเกี่ยวกับ reward,
	        redeem_points:  คะแนนที่ต้องใช้ในการแลก,
	        stocks: จำนวนของ reward ที่มีทั้งหมด
        }
 - การลบของรางวัล (เฉพาะผู้ดูแลระบบเท่านั้น): `Delete /rewards/:id`     
 
        headers: {
	        Authorization: `Bearer ${JWT}`
	    }
	    Params: Integer:id ของ reward ที่ต้องการลบ
**/score-histories**      
- ดูประวัติคะแนนการได้แต้มหรือใช้แต้มจากการส่งงานของนักเรียนทั้งหมด: `GET /score-histories`      
- ดูประวัติคะแนนการได้แต้มหรือใช้แต้มจากการส่งงานของนักเรียน: `GET /score-histories?student.std_id`     
`Query String: std_id=รหัสนักเรียนที่เป็น running number`     

 