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


-Cau 6-
+Nhập thư viện
import numpy as np: 
import imageio.v2 as iio 
import scipy.ndimage as sn
import os
import matplotlib.pylab as plt

+Đặt tên thư mục chứa các hình ảnh gốc
input_folder = 'Exercise'
+Đặt tên thư mục đầu ra nơi lưu các hình ảnh đã qua xử lý (khử nhiễu). Các ảnh đã qua lọc sẽ được lưu vào thư mục này.
output_folder = 'Exercise_Filtered'
+Tạo thư mục Exercise_Filtered nếu chưa tồn tại
os.makedirs(output_folder, exist_ok=True):
+Tạo một bộ lọc Mean với kích thước 5x5
mean_filter_kernel = np.ones((5, 5)) / 25
+Duyệt qua tất cả các tệp tin trong thư mục Exercise
for filename in os.listdir(input_folder): 
+Tạo đường dẫn đầy đủ đến tệp ảnh hiện tại.
input_path = os.path.join(input_folder, filename): Tạo đường dẫn đầy đủ đến tệp ảnh hiện tại.
+Đọc ảnh từ input_path và chuyển ảnh thành ảnh xám (grayscale) bằng as_gray=True. Sau đó, chuyển đổi kiểu dữ liệu của ảnh thành np.uint8
img = iio.imread(input_path, as_gray=True).astype(np.uint8)
+Kiểm tra ảnh có hợp lệ hay không
if img is not None

Áp dụng các bộ lọc và lưu ảnh đã áp dụng bộ lọc

Mean Filter:

mean_filtered_img = sn.convolve(img, mean_filter_kernel).astype(np.uint8)

iio.imsave(os.path.join(output_folder, f"mean_{filename}"), mean_filtered_img)
Median Filter:

median_filtered_img = sn.median_filter(img, size=5, mode='reflect') 

iio.imsave(os.path.join(output_folder, f"median_{filename}"), median_filtered_img)

Max Filter:

max_filtered_img = sn.maximum_filter(img, size=5, mode='reflect')

iio.imsave(os.path.join(output_folder, f"max_{filename}"), max_filtered_img)

Min Filter:

min_filtered_img = sn.minimum_filter(img, size=5, mode='reflect')

iio.imsave(os.path.join(output_folder, f"min_{filename}"), min_filtered_img)

-Cau7-

+Nhập thư viện
import numpy as np
import imageio.v2 as iio
import scipy.ndimage as sn
from skimage import feature, filters
import os
import matplotlib.pylab as plt

+Tạo thư mục đầu ra
input_folder = 'exercise'
output_folder = 'Exercise_cau7'

+Bộ lọc Mean và Prewitt
mean_filter_kernel = np.ones((5, 5)) / 25
prewitt_x = np.array([[ -1, 0, 1], [-1, 0, 1], [-1, 0, 1]])
prewitt_y = np.array([[ -1, -1, -1], [ 0,  0,  0], [ 1,  1,  1]])

+Đọc và xử lý hình ảnh trong thư mục
for filename in os.listdir(input_folder):
    input_path = os.path.join(input_folder, filename)
    img = iio.imread(input_path, as_gray=True).astype(np.uint8)

+Khử nhiễu với Mean Filter
denoised_img = sn.convolve(img, mean_filter_kernel).astype(np.uint8)

+Áp dụng các bộ lọc biên và lưu kết quả

sobel_edges = filters.sobel(denoised_img)
iio.imsave(os.path.join(output_folder, f"sobel_edges_{filename}"), sobel_edges)
-Áp dụng bộ lọc Sobel để phát hiện biên. Bộ lọc Sobel tính gradient của ảnh theo các hướng ngang và dọc.


canny_edges = feature.canny(denoised_img, sigma=3)
iio.imsave(os.path.join(output_folder, f"canny_edges_{filename}"), canny_edges)
-Áp dụng bộ lọc Canny để phát hiện biên. Tham số sigma=3 giúp làm mờ ảnh trước khi phát hiện biên để giảm nhiễu.


grad_x = sn.convolve(denoised_img, prewitt_x)
grad_y = sn.convolve(denoised_img, prewitt_y)
prewitt_edges = np.hypot(grad_x, grad_y)
prewitt_edges = np.uint8(prewitt_edges / np.max(prewitt_edges) * 255)
iio.imsave(os.path.join(output_folder, f"prewitt_edges_{filename}"), prewitt_edges)
- Áp dụng bộ lọc Prewitt theo các hướng ngang và dọc để tính gradient.
- Kết hợp các gradient để tính toán tổng gradient (biên).
- Chuẩn hóa ảnh biên vào phạm vi [0, 255].


laplace_edges = sn.laplace(denoised_img, mode='reflect')
iio.imsave(os.path.join(output_folder, f"laplace_edges_{filename}"), laplace_edges)
-Áp dụng bộ lọc Laplace để phát hiện biên, dựa trên đạo hàm bậc hai.

-Cau8-

