Lab1
-Cau1
+Sử dụng thư viện Pillow để thao tác với ảnh
from PIL import Image

+import ảnh từ folder vào để thực hiện chỉnh sửa
img = Image.open("bird.png")

+tách các kênh màu ra 
r, g, b = img.split()

+Tạo ảnh đen để thay thế cho các kênh không sử dụng màu
black = Image.new("L", img.size)

+Tạo màu cho ảnh, với r là red, g là green, b là blue
red_img = Image.merge("RGB", (r, black, black))
green_img = Image.merge("RGB", (black, g, black))
blue_img = Image.merge("RGB", (black, black, b))

+Lưu ảnh ra các file mới
red_img.save("anh_mau_do.png")
green_img.save("anh_mau_luc.png")
blue_img.save("anh_mau_lam.png")

-Cau2
+Sử dụng thư viện Pillow để thao tác với ảnh
from PIL import Image

+import ảnh từ folder vào để thực hiện chỉnh sửa
img = Image.open("bird.png")

+tách các kênh màu ra 
r, g, b = img.split()

+Hoán đổi các kênh màu lần lượt với 3 kênh red, green, blue
img_rg = Image.merge("RGB", (g, r, b)) 
img_gb = Image.merge("RGB", (r, b, g))  
img_rb = Image.merge("RGB", (b, g, r))

+Lưu ảnh ra các file mới
img_rg.save("anh_doi_R_G.png")
img_gb.save("anh_doi_G_B.png")
img_rb.save("anh_doi_R_B.png")

-Cau3
+Nhập thư viên
import cv2
import numpy as np

+Đọc ảnh lấy từ folder
img = cv2.imread("bird.png") 

+Chuyển ảnh từ hệ màu BGR sang HSV
hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV) 

+Tách thành các kênh riêng biệt
h, s, v = cv2.split(hsv_img)

+Tạo ảnh HSV với các màu riêng biệt
hue_img = cv2.merge([h, np.full_like(h, 255), np.full_like(h, 255)]) #Giữ Hue và đặt S = 255, V = 255 để làm nổi bật rõ sắc màu
hue_bgr = cv2.cvtColor(hue_img, cv2.COLOR_HSV2BGR)

sat_img = cv2.merge([np.zeros_like(s), s, np.full_like(s, 255)]) #Giữ Saturation và đặt H = 0 (tức màu đỏ), V = 255 (sáng nhất) để làm nổi bật độ rực màu
sat_bgr = cv2.cvtColor(sat_img, cv2.COLOR_HSV2BGR)

val_img = cv2.merge([np.zeros_like(v), np.zeros_like(v), v]) #Giữ Value và đặt H = 0, S = 0 để thể hiện rõ độ sáng tối của ảnh
val_bgr = cv2.cvtColor(val_img, cv2.COLOR_HSV2BGR)

+Lưu thành ảnh mới
cv2.imwrite("kenh_Hue.png", hue_bgr)
cv2.imwrite("kenh_Saturation.png", sat_bgr)
cv2.imwrite("kenh_Value.png", val_bgr)

-Cau4

+Nhập thư viên
import cv2
import numpy as np

+Đọc ảnh lấy từ folder
img = cv2.imread("bird.png")

+Chuyển ảnh từ hệ màu BGR sang HSV
hsv_img = cv2.cvtColor(img, cv2.COLOR_BGR2HSV) 

+Tách thành các kênh riêng biệt
h, s, v = cv2.split(hsv_img)

+Tính Hue mới và Value mới cho ảnh lần lượt với Hnew = 1/3 Hold và Vnew = 3/4 Vold
h_new = np.clip((h.astype(np.float32) / 3), 0, 179).astype(np.uint8)
v_new = np.clip((v.astype(np.float32) * 0.75), 0, 255).astype(np.uint8)

+Ghép 3 kênh HSV với Hnew và Vnew lại
new_hsv = cv2.merge([h_new, s, v_new])

+Chuyển lại thành BGR để hiển thị ảnh
output_img = cv2.cvtColor(new_hsv, cv2.COLOR_HSV2BGR)

+Lưu thành ảnh mới
cv2.imwrite("anh_HSV_chinh_sua.jpg", output_img)

-Cau5

+Nhập thư viên
import cv2
import numpy as np

+Nhập folder input và output của ảnh
input_folder = "exercise"
output_folder = "Exercise_Filtered_Mean"

+Tạo thư mục Exercise_Filtered_Mean nếu chưa tồn tại.
os.makedirs(output_folder, exist_ok=True)

+Duyệt qua tất cả các file trong thư mục exercise.
for filename in os.listdir(input_folder)

+Tạo đường dẫn đầy đủ đến ảnh gốc
input_path = os.path.join(input_folder, filename)

+Tạo đường dẫn để lưu ảnh sau xử lý và thêm "mean_" đằng trước để phân biệt ảnh đã được thêm meanfilter
output_path = os.path.join(output_folder, f"mean_{filename}")

+Đọc ảnh
img = cv2.imread(input_path)

+Sử dụng hàm if để đọc ảnh và áp dụng meanfilter nếu không đọc được sẽ xuất ra dòng lỗi
 if img is not None:
            filtered = cv2.blur(img, (5, 5))
            cv2.imwrite(output_path, filtered)
            print(f"Đã xử lý: {filename}")
        else:
            print(f"Lỗi: Không thể đọc {filename}")

