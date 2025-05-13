# BaiTapCaNhan
# 1. Mục tiêu
Trang readme này trình bày mô tả chi tiết về các thuật toán tìm kiếm được triển khai trong mã nguồn Python để giải quyết bài toán 8-puzzle. Mỗi thuật toán được phân tích dựa trên ý tưởng cốt lõi, cơ chế hoạt động, các đặc điểm về tính hoàn chỉnh, tối ưu và độ phức tạp thuật toán ước lượng.

# 2. Nội dung
# 2.1. Các thuật toán Tìm kiếm không có thông tin (Uninformed Search Algorithms)
Một bài toán tìm kiếm thường bao gồm các thành phần sau:

Không gian trạng thái (State Space): Tập hợp tất cả các trạng thái có thể đạt được. Đối với bài toán 8-puzzle, mỗi cách sắp xếp các ô là một trạng thái. Tổng số trạng thái là 9!/2=181,440.  

Trạng thái ban đầu (Initial State): Trạng thái xuất phát của bài toán.

Hành động (Actions): Các thao tác có thể thực hiện để chuyển từ trạng thái này sang trạng thái khác (ví dụ: di chuyển ô trống lên, xuống, trái, phải).

Mô hình chuyển đổi (Transition Model): Mô tả trạng thái kết quả khi thực hiện một hành động ở một trạng thái cụ thể.

Kiểm tra đích (Goal Test): Xác định xem một trạng thái có phải là trạng thái đích (trạng thái mong muốn) hay không.

Chi phí đường đi (Path Cost): Hàm gán một chi phí số cho một đường đi. Trong bài toán 8-puzzle cơ bản, mỗi bước di chuyển có thể coi là chi phí bằng 1.

Giải pháp (Solution): Là một chuỗi các hành động, gọi là đường đi (path), từ trạng thái ban đầu đến một trạng thái đích. Một giải pháp tối ưu là giải pháp có chi phí đường đi thấp nhất.

Các tham số độ phức tạp:

•	b: Hệ số nhánh (tối đa 4 cho 8-puzzle).

•	d: Độ sâu của lời giải nông nhất.

•	m: Độ sâu tối đa của không gian trạng thái (khoảng 31 cho 8-puzzle).	

•	V: Tổng số trạng thái (V=9! / 2 = 181 440 cho 8-puzzle).

•	E: Tổng số bước chuyển trạng thái.

•	C*: Chi phí của lời giải tối ưu (bằng d khi chi phí mỗi bước là 1).

•	epsilon: Chi phí bước nhỏ nhất (bằng 1).

# Thuật toán IDS     
Ý tưởng cốt lõi: Kết hợp BFS (tối ưu) và DFS (bộ nhớ thấp) bằng cách thực hiện DLS (Depth-Limited Search) với giới hạn độ sâu tăng dần. 

Cách hoạt động: Lặp DLS với độ sâu 0, 1, 2, ... cho đến khi tìm thấy lời giải. Mỗi lần DLS là một DFS bị giới hạn độ sâu. 

Đặc điểm: 

  Hoàn chỉnh: Có.
  
  Tối ưu: Có (về số bước).
Độ phức tạp (ước lượng):

  Thời gian: O(b^d).
  
  Không gian: O(bd).

Nhận xét về hiệu suất: IDS kết hợp được ưu điểm của BFS (tính tối ưu về số bước, tính hoàn chỉnh) và DFS (yêu cầu bộ nhớ thấp). Mặc dù nó duyệt lại các nút nhiều lần, chi phí tổng thể không quá lớn so với BFS.
![IDS](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/IDS_new.gif)

# Thuật toán BFS     
Ý tưởng cốt lõi: Khám phá trạng thái theo từng lớp độ sâu, đảm bảo tìm thấy đường đi ngắn nhất về số bước. 

Cách hoạt động: Sử dụng hàng đợi (Queue - FIFO). Thăm tất cả các nút ở độ sâu k trước khi thăm bất kỳ nút nào ở độ sâu k+1. Sử dụng tập visited để tránh duyệt lại trạng thái. 

Đặc điểm: 

  Hoàn chỉnh (Complete): Có.
  
  Tối ưu (Optimal): Có (về số bước đi).
  
Độ phức tạp (ước lượng): 

  Thời gian: O(b^d).
  
  Không gian: O(b^d).
  
Nhận xét về hiệu suất: BFS đảm bảo tìm ra lời giải ngắn nhất (nếu chi phí bước là như nhau). Tuy nhiên, yêu cầu về bộ nhớ và thời gian có thể rất lớn nếu lời giải ở độ sâu lớn.
![BFS](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/BFS_new.gif)


# Thuật toán UCS          
Ý tưởng cốt lõi: Mở rộng nút có chi phí đường đi (g) thấp nhất tính từ nút bắt đầu. Tương đương BFS khi chi phí mọi hành động là 1. 

Cách hoạt động: Sử dụng hàng đợi ưu tiên (Priority Queue) sắp xếp theo chi phí g. Luôn chọn nút có g nhỏ nhất để mở rộng. Sử dụng tập visited. 

Đặc điểm: 

  Hoàn chỉnh: Có.
  
  Tối ưu: Có (tìm đường đi với tổng chi phí thấp nhất).
  
Độ phức tạp (ước lượng): (Với chi phí bước là 1, C* = d) 

  Thời gian: O(b^d) hoặc O(b^(1 + floor(C*/epsilon))). Tương tự BFS trong trường hợp này. Có thể lên đến O(V log V) nếu xét chi phí hàng đợi ưu tiên.
  
  Không gian: O(b^d).

Nhận xét về hiệu suất: UCS luôn tìm ra đường đi có chi phí thấp nhất. Giống như BFS, nó có thể gặp vấn đề về thời gian và không gian nếu không gian tìm kiếm lớn.
![UCS](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/UCS_new.gif)

# Thuật toán DFS     
Ý tưởng cốt lõi: Khám phá sâu nhất có thể theo một nhánh trước khi quay lui (backtrack). 

Cách hoạt động: Sử dụng ngăn xếp (Stack - LIFO) hoặc đệ quy. Đi sâu vào một nhánh cho đến khi gặp nút lá hoặc đích, sau đó quay lại và thử nhánh khác. Sử dụng tập visited để tránh chu trình vô hạn. 

Đặc điểm: 

  Hoàn chỉnh: Có (vì không gian trạng thái 8-puzzle hữu hạn và có kiểm tra visited).
  
  Tối ưu: Không.
Độ phức tạp (ước lượng): 

  Thời gian: O(V) hoặc O(b^m) trong trường hợp xấu nhất. Với visited, giới hạn bởi số trạng thái V.
  
  Không gian: O(bm) cho ngăn xếp/đệ quy, nhưng có thể lên tới O(V) nếu tính cả tập visited. Thường coi là tiết kiệm bộ nhớ hơn BFS về lưu trữ đường đi.

Nhận xét về hiệu suất: DFS có yêu cầu bộ nhớ ít hơn BFS. Tuy nhiên, nó không đảm bảo tìm ra lời giải tối ưu và có thể đi vào các nhánh rất sâu không cần thiết.
![DFS](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/DFS_new.gif)

# 2.2. Các thuật toán Tìm kiếm có thông tin (Informed Search Algorithms)
# Thuật toán IDA*      
Ý tưởng cốt lõi: Kết hợp A* và IDS. Thực hiện tìm kiếm giới hạn "chi phí f" thay vì giới hạn độ sâu. Tối ưu và tiết kiệm bộ nhớ.

Cách hoạt động: Đặt ngưỡng chi phí f. Thực hiện tìm kiếm kiểu DFS, cắt nhánh nếu f(n) > threshold. Nếu không tìm thấy, tăng ngưỡng và lặp lại. 

Đặc điểm: 

  Hoàn chỉnh: Có.
  
  Tối ưu: Có (với heuristic admissible).
Độ phức tạp (ước lượng): 

  Thời gian: O(b^d). Tương tự A* về số nút mở rộng.
  
  Không gian: O(bd). Rất tiết kiệm bộ nhớ.

Nhận xét về hiệu suất: IDA* đạt được tính tối ưu của A* trong khi chỉ yêu cầu bộ nhớ tương đương với DFS. Đây là một lựa chọn tốt khi bộ nhớ là một hạn chế.
![IDAstar](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/IDA_star_new.gif)

# Thuật toán A*     
Ý tưởng cốt lõi: Kết hợp UCS và Greedy Search. Mở rộng nút có tổng chi phí ước lượng f(n) = g(n) + h(n) thấp nhất.

Cách hoạt động: Sử dụng hàng đợi ưu tiên sắp xếp theo f(n). Luôn chọn nút có f nhỏ nhất để mở rộng. Sử dụng visited. 
Đặc điểm: 

  Hoàn chỉnh: Có.
  
  Tối ưu: Có (nếu heuristic h là chấp nhận được - admissible).
  
Độ phức tạp (ước lượng): 

  Thời gian: Phụ thuộc mạnh vào heuristic. Từ O(V log V) đến O(b^d) (trường hợp xấu).
  
  Không gian: O(b^d) hoặc O(V). Thường là hạn chế lớn nhất của A*.

Nhận xét về hiệu suất: A* là một trong những thuật toán tìm kiếm tối ưu hiệu quả nhất nếu có hàm heuristic tốt. Tuy nhiên, nó đòi hỏi nhiều bộ nhớ để lưu trữ các nút đã mở rộng.
![Astar](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/A_star_new.gif)

# Thuật toán Greedy Search    
Ý tưởng cốt lõi: Luôn mở rộng nút có vẻ gần đích nhất theo hàm heuristic h (Manhattan distance), bỏ qua chi phí đã đi g. 

Cách hoạt động: Sử dụng hàng đợi ưu tiên sắp xếp theo h(n). Luôn chọn nút có h nhỏ nhất để mở rộng. Sử dụng visited. 

Đặc điểm: 

  Hoàn chỉnh: Không.
  
  Tối ưu: Không.
  
Độ phức tạp (ước lượng): 

  Thời gian: Phụ thuộc mạnh vào heuristic. Xấu nhất có thể là O(V) hoặc O(b^m).
  
  Không gian: Phụ thuộc mạnh vào heuristic. Xấu nhất có thể là O(V) hoặc O(b^m).

Nhận xét về hiệu suất: Thường tìm ra lời giải nhanh chóng nhưng không đảm bảo tối ưu hoặc thậm chí không tìm ra lời giải (có thể bị kẹt). Hiệu quả phụ thuộc lớn vào chất lượng của hàm heuristic.
![GreedySearch](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/GreedySearch_new.gif)

# 2.3. Tìm kiếm cục bộ (Local Search)
# Thuật toán Simple Hill Climbing  
Ý tưởng cốt lõi: Tìm kiếm cục bộ. Di chuyển đến trạng thái hàng xóm đầu tiên tốt hơn (heuristic h thấp hơn) trạng thái hiện tại. 

Cách hoạt động: Lặp: tìm hàng xóm, nếu có hàng xóm tốt hơn đầu tiên thì chuyển, nếu không thì dừng (kẹt). 

Đặc điểm: 

  Hoàn chỉnh: Không. Dễ bị kẹt ở cực trị cục bộ hoặc bình nguyên.
  
  Tối ưu: Không.
  
Độ phức tạp (ước lượng): 

  Thời gian: Nhanh cho mỗi bước, không đảm bảo thời gian tìm ra lời giải.
  
  Không gian: O(1).

Nhận xét về hiệu suất: Thuật toán leo đồi này rất nhanh và tiết kiệm bộ nhớ nhưng thường không tìm được lời giải tối ưu và dễ bị kẹt.
![SimpleHill](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/SimpleHill_new.gif)

# Thuật toán Hill Climbing  
Ý tưởng cốt lõi: Tìm kiếm cục bộ. Xem xét tất cả hàng xóm và di chuyển đến hàng xóm tốt nhất (có h thấp nhất). 

Cách hoạt động: Lặp: đánh giá tất cả hàng xóm, chọn hàng xóm tốt nhất (nếu tốt hơn hiện tại) để di chuyển, nếu không thì dừng. 

Đặc điểm: Giống Simple Hill Climbing nhưng ít bị ảnh hưởng bởi thứ tự hàng xóm hơn. Vẫn bị kẹt.

Độ phức tạp (ước lượng): 

  Thời gian: Mỗi bước tốn O(b), không đảm bảo thời gian tìm ra lời giải.
  
  Không gian: O(1).
Nhận xét về hiệu suất: Tương tự Simple Hill Climbing thì thuật toán leo đồi Hill Climbing cũng rất nhanh và tiết kiệm bộ nhớ nhưng thường không tìm được lời giải tối ưu và dễ bị kẹt.
![HillCLimbing](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/HillClimbing_1.gif)

# Thuật toán Stochastic Climbing
Ý tưởng cốt lõi: Tìm kiếm cục bộ. Chọn ngẫu nhiên một hàng xóm, nếu tốt hơn thì di chuyển, nếu không thì thử hàng xóm ngẫu nhiên khác. 

Cách hoạt động: Lặp: chọn ngẫu nhiên hàng xóm, nếu tốt hơn thì chuyển, nếu không thì bỏ qua và thử lại. Dừng nếu hết lựa chọn tốt hơn. 

Đặc điểm: Không hoàn chỉnh, không tối ưu. Tính ngẫu nhiên có thể giúp khám phá, nhưng vẫn có thể bị kẹt. 

Độ phức tạp (ước lượng): 

  Thời gian: Không xác định, phụ thuộc may mắn.
  
  Không gian: O(1).

Nhận xét về hiệu suất: Tương tự như các thuật toán leo đồi khác, nhanh và tiết kiệm bộ nhớ nhưng không đảm bảo tìm ra giải pháp tốt.
![Stochastic](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/StochasticClimbing.gif)

# Thuật toán Beam Search
Ý tưởng cốt lõi: Biến thể của Best-First Search. Chỉ giữ lại k (beam width) trạng thái tốt nhất (theo heuristic h) ở mỗi bước để mở rộng tiếp, nhằm hạn chế bộ nhớ. 

Cách hoạt động: Lặp: mở rộng k nút tốt nhất, tạo con cháu, chọn ra k con cháu tốt nhất cho bước tiếp theo. 

Đặc điểm: 
  Hoàn chỉnh: Không.
  
  Tối ưu: Không.
  
Độ phức tạp (ước lượng): 

  Thời gian: O(k * b * d) hoặc tương tự. Nhanh hơn BFS/A*.
  
  Không gian: O(k * b). Bị giới hạn bởi k.

Nhận xét về hiệu suất: Beam Search là một sự thỏa hiệp giữa hiệu quả và tính đầy đủ/tối ưu. Nó có thể tìm ra giải pháp tốt một cách nhanh chóng với bộ nhớ hạn chế, nhưng có thể bỏ lỡ giải pháp tối ưu.
![BeamSearch](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/BeamSearch.gif)

# Thuật toán Simulated Annealing
Ý tưởng cốt lõi: Thuật toán tối ưu hóa xác suất. Cho phép di chuyển đến trạng thái tệ hơn với xác suất giảm dần theo "nhiệt độ" T, giúp thoát khỏi cực trị cục bộ. 

Cách hoạt động: Lặp: chọn hàng xóm ngẫu nhiên. Nếu tốt hơn, di chuyển. Nếu tệ hơn, di chuyển với xác suất exp(delta_E / T). Giảm T theo thời gian. 

Đặc điểm: Có khả năng tìm ra giải pháp tốt nếu làm nguội đủ chậm, nhưng không đảm bảo. 

Độ phức tạp (ước lượng): 

Thời gian: Phụ thuộc vào lịch trình làm nguội.

Không gian: O(1).

Nhận xét về hiệu suất: Simulated Annealing có thể thoát khỏi các cực tiểu cục bộ và tìm ra giải pháp gần tối ưu toàn cục, nhưng hiệu suất phụ thuộc vào việc lựa chọn các tham số (lịch trình làm nguội).
![SimulatedAnnealing](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/SimulatedAnnealing.gif)

# Thuật toán Genetic Algorithm
Ý tưởng cốt lõi: Mô phỏng tiến hóa. Duy trì quần thể trạng thái, áp dụng chọn lọc, lai ghép, đột biến qua các thế hệ để tìm trạng thái tốt nhất (fitness cao - h thấp). 

Cách hoạt động: Tối ưu hóa trạng thái, không phải đường đi. Lặp qua các thế hệ, đánh giá fitness, chọn lọc, lai ghép (crossover), đột biến (mutate) để tạo thế hệ mới. 

Đặc điểm: Metaheuristic, không đảm bảo hoàn chỉnh/tối ưu. Tìm trạng thái đích nhưng không trả về đường đi. 

Độ phức tạp (ước lượng): 

  Thời gian: O(generations * population_size * cost_per_individual).
  
  Không gian: O(population_size).

Nhận xét về hiệu suất: Thuật toán di truyền có thể hiệu quả cho các không gian tìm kiếm phức tạp, nhưng việc thiết kế các toán tử di truyền và hàm fitness phù hợp là rất quan trọng. Nó thường dùng để tìm trạng thái tốt chứ không phải đường đi.
![GeneticAlgorithm](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/GeneticAlgorithm.gif)

# 2.4. Tìm kiếm trong môi trường phức tạp
# Thuật toán And Or Search 
Ý tưởng cốt lõi: Dùng cho bài toán phân rã thành bài toán con (AND nodes, OR nodes). 

Cách hoạt động: Thực chất là DFS/Backtracking với giới hạn độ sâu và kiểm tra chu trình cục bộ. Không thể hiện cấu trúc AND-OR thực sự cho 8-puzzle. 

Đặc điểm: Giống DFS có giới hạn độ sâu. 

Độ phức tạp (ước lượng): 

  Thời gian: O(b^M) với M là max_depth.
  
  Không gian: O(bM) cho ngăn xếp đệ quy.
![AndOrSearch](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/AndOrSearch.gif)

# Thuật toán Belief Search
Ý tưởng cốt lõi: Tìm kiếm trong không gian các "trạng thái niềm tin" (tập hợp các trạng thái vật lý có thể) khi trạng thái hiện tại không chắc chắn. 

Cách hoạt động: Thực hiện BFS trên không gian trạng thái niềm tin. Một hành động biến đổi toàn bộ tập trạng thái hiện tại thành tập trạng thái mới. Đích đạt được khi mọi trạng thái trong tập đều là đích. 

Đặc điểm: Giải quyết bài toán trong môi trường không chắc chắn/quan sát một phần. 

Độ phức tạp (ước lượng): 

  Thời gian: Có thể là O(2^V). Rất cao.
  
  Không gian: Có thể là O(2^V).
![BeliefSearch](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/Belief_Search_ver3.gif)

# Thuật toán Partial Observations
Ý tưởng cốt lõi: Tương tự belief_search, xử lý việc không biết chắc trạng thái hiện tại.

Cách hoạt động: Giống belief_search, duy trì và cập nhật tập hợp các trạng thái có thể (belief state). Có thêm giới hạn max_steps.

Đặc điểm: Giống belief_search.

Độ phức tạp (ước lượng): 

  Thời gian: Lũy thừa theo V, có thể lên tới O(2^V).
  
  Không gian: Lũy thừa theo V, có thể lên tới O(2^V).
![PartialObservations](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/PatialObservations_ver3.gif)

# 2.5. Tìm kiếm trong môi trường có ràng buộc (Constraint Satisfaction Problems - CSPs)
# Thuật toán Backtracking Search
Ý tưởng cốt lõi: Xây dựng giải pháp từng bước, nếu gặp ngõ cụt, quay lại bước trước và thử lựa chọn khác. 

Cách hoạt động: Sử dụng đệ quy để khám phá đường đi. Thêm trạng thái mới nếu hợp lệ và chưa có trong đường đi hiện tại. Nếu đạt đích, trả về. Nếu hết lựa chọn hoặc đạt giới hạn độ sâu, quay lui. 

Đặc điểm: Là một dạng DFS. 

  Hoàn chỉnh: Có (nếu max_depth đủ lớn).
  
  Tối ưu: Không.
  
Độ phức tạp (ước lượng): 

  Thời gian: O(b^M) với M là max_depth. Tương tự DFS.
  
  Không gian: O(M). Chỉ cần lưu đường đi hiện tại.
![BacktrackingSearch](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/BacktrackingSearch_ver2.gif)

# Thuật toán Forward Checking
Ý tưởng cốt lõi: Khi một giá trị được gán cho một biến trong quá trình tìm kiếm (thường là trong Backtracking Search), thuật toán này sẽ "nhìn về phía trước" (forward) để kiểm tra các biến chưa được gán có ràng 

buộc với biến vừa được gán. Nó loại bỏ các giá trị không tương thích khỏi miền giá trị của các biến chưa được gán đó, giúp phát hiện sớm các ngõ cụt.

Cách hoạt động:
Đầu tiên, khi một biến Xi được gán một giá trị v (ví dụ, trong một bước của thuật toán Backtracking).

Thứ hai, đối với mỗi biến Xj chưa được gán mà có ràng buộc với Xi: Loại bỏ khỏi miền giá trị Dj của Xj tất cả các giá trị không nhất quán với việc Xi=v.

Thứ ba, nếu việc loại bỏ này làm cho miền giá trị của bất kỳ biến Xj nào trở thành rỗng, điều đó có nghĩa là phép gán hiện tại (Xi =v) dẫn đến ngõ cụt. Thuật toán sẽ backtrack (nếu đang kết hợp với Backtracking).

Cuối cùng, khi một phép gán bị hủy bỏ (backtrack), các giá trị đã bị loại bỏ khỏi miền của các biến khác do phép gán đó phải được khôi phục.

Đặc điểm:

Hoàn chỉnh: Có (khi được sử dụng kết hợp với một thuật toán tìm kiếm hoàn chỉnh như Backtracking). Nó không tự mình là một thuật toán tìm kiếm hoàn chỉnh mà là một kỹ thuật tăng cường.

Tối ưu: Không áp dụng trực tiếp. Mục tiêu của Forward Checking là tăng hiệu quả của thuật toán tìm kiếm bằng cách cắt tỉa không gian tìm kiếm, giúp tìm ra giải pháp (bất kỳ giải pháp nào thỏa mãn ràng buộc) nhanh hơn, chứ không phải để tìm giải pháp tối ưu theo một tiêu chí nào đó.

Độ phức tạp (ước lượng):

Thời gian: Thường cải thiện đáng kể hiệu suất trung bình của Backtracking Search bằng cách giảm số lượng các nút phải duyệt. Tuy nhiên, trong trường hợp xấu nhất, độ phức tạp vẫn có thể là hàm mũ so với kích thước bài toán. Chi phí cho mỗi lần gán biến tăng lên do phải kiểm tra các biến liên quan.

Không gian: Cần thêm không gian để lưu trữ các miền giá trị hiện tại của các biến, có thể bị thay đổi trong quá trình tìm kiếm. Thường là O(nd) với n là số biến và d là kích thước miền giá trị lớn nhất.
# Thuật toán AC-3
Ý tưởng cốt lõi: Đảm bảo tính nhất quán cung (arc consistency) cho tất cả các "cung" (arc) trong bài toán thỏa mãn ràng buộc (CSP). Một cung (Xi, Xj) được coi là nhất quán nếu với mọi giá trị x trong miền của biến Xi, tồn tại ít nhất một giá trị y trong miền của biến Xj sao cho cặp giá trị (x,y) thỏa mãn ràng buộc giữa Xi và Xj. AC-3 loại bỏ các giá trị không thỏa mãn điều kiện này khỏi miền của các biến.

Cách hoạt động: 
Thứ nhất, khởi tạo một tập hợp (thường là hàng đợi) chứa tất cả các cung (Xi​, Xj) trong CSP.

Thứ hai, trong khi tập hợp cung không rỗng: Đầu tiên, ta lấy một cung (Xi, Xj) ra khỏi tập hợp. Sau đó, đối với mỗi giá trị x trong miền Di của Xi: * Nếu không tồn tại giá trị y nào trong miền Dj của Xj sao cho (x,y) thỏa mãn ràng buộc giữa Xi và Xj, thì loại bỏ x khỏi Di. Cuối cùng, nếu Di bị thay đổi (có giá trị bị loại bỏ): * Nếu Di trở thành rỗng, bài toán không có giải pháp. Dừng lại. * Thêm tất cả các cung (Xk, Xi) vào tập hợp (với Xk là các biến "hàng xóm" của Xi, tức là có ràng buộc với Xi, và X​k khác Xj). Điều này là cần thiết vì việc thay đổi Di có thể ảnh hưởng đến tính nhất quán của các cung khác liên quan đến Xi.

Thứ ba, lặp lại cho đến khi tập hợp cung rỗng (không còn cung nào cần kiểm tra lại).

Đặc điểm:

Hoàn chỉnh: AC-3 không phải là một thuật toán tìm kiếm giải pháp hoàn chỉnh. Nó là một thuật toán suy luận ràng buộc (hoặc lan truyền ràng buộc). Sau khi AC-3 chạy, CSP có thể vẫn cần một thuật toán tìm kiếm (như Backtracking) để tìm giải pháp. Tuy nhiên, AC-3 giúp giảm kích thước miền của các biến, có thể làm cho việc tìm kiếm sau đó hiệu quả hơn đáng kể hoặc thậm chí xác định được rằng không có giải pháp.

Tối ưu: Không áp dụng. Mục tiêu là làm cho CSP nhất quán cung.

Độ phức tạp (ước lượng):

Thời gian: O(c.(d^3))  hoặc hiệu quả hơn là O(c⋅(d^2)) với một số cách triển khai tối ưu, trong đó c là số lượng cung (ràng buộc hai ngôi) và d là kích thước tối đa của miền giá trị của các biến.

Không gian: O(c+nd) để lưu trữ các cung, các miền giá trị của biến, và cấu trúc dữ liệu cho hàng đợi, với n là số biến.
# 2.6. Học tăng cường (Reinforcement Learning)
# Q-Learning
Ý tưởng cốt lõi: Q-Learning là một thuật toán học tăng cường không cần mô hình (model-free) và ngoài chính sách (off-policy). Mục tiêu của nó là học một hàm giá trị hành động, gọi là hàm Q (Q-function), Q(s,a). Hàm này ước tính tổng phần thưởng chiết khấu kỳ vọng trong tương lai khi thực hiện hành động a tại trạng thái s và sau đó tuân theo chính sách tối ưu. Bằng cách học được hàm Q∗(s,a) tối ưu, agent có thể xác định được hành động tốt nhất tại mỗi trạng thái.

Cách hoạt động:

Thứ nhất, khởi tạo một bảng Q (Q-table) để lưu trữ các giá trị Q(s,a) cho tất cả các cặp (trạng thái s, hành động a). Các giá trị này thường được khởi tạo bằng 0 hoặc một giá trị tùy ý nhỏ.

Thứ hai, lặp (cho mỗi bước thời gian hoặc mỗi episode): 

a. Quan sát trạng thái hiện tại s. 

b. Chọn hành động a: Chọn một hành động a tại trạng thái s. Việc lựa chọn này thường dựa trên chính sách ϵ-greedy: * Với xác suất ϵ (epsilon), chọn một hành động ngẫu nhiên (khám phá - exploration). * Với xác suất 1−ϵ, chọn hành động a có giá trị Q (s, a) lớn nhất (khai thác - exploitation): a = argmaxa′Q (s, a′). 

c. Thực hiện hành động a: Agent thực hiện hành động a và chuyển đến trạng thái mới s′. Quan sát phần thưởng r nhận được từ môi trường và trạng thái kế tiếp s′. 

d. Cập nhật giá trị Q: Cập nhật giá trị Q(s,a) trong Q-table bằng công thức cập nhật của Q-Learning: Q(s,a)←Q(s,a)+α[r+γmaxa′Q(s′,a′) − Q(s,a)] Trong đó: * α (alpha) là tốc độ học (learning rate, 0<α≤1): Xác định mức độ mà thông tin mới ghi đè lên thông tin cũ. * γ (gamma) là yếu tố chiết khấu (discount factor, 0≤γ<1): Thể hiện tầm quan trọng của các phần thưởng trong tương lai. Giá trị γ gần 0 khiến agent tập trung vào phần thưởng trước mắt, trong khi giá trị gần 1 khiến agent hướng tới phần thưởng dài hạn. * r là phần thưởng nhận được sau khi thực hiện hành động a tại trạng thái s. * s′ là trạng thái tiếp theo. * maxa′Q(s′,a′) là ước tính của giá trị tối ưu trong tương lai, tức là giá trị Q lớn nhất có thể đạt được từ trạng thái s′ bằng cách chọn hành động a′ tốt nhất. e. Cập nhật trạng thái: s←s′.
Thứ ba, hội tụ quá trình lặp lại cho đến khi Q-table hội tụ (các giá trị Q thay đổi rất ít sau mỗi lần cập nhật) hoặc sau một số lượng lớn các episode/bước.

Đặc điểm:

Hoàn chỉnh (Hội tụ đến giá trị Q tối ưu): Có. Q-Learning được chứng minh là sẽ hội tụ đến hàm giá trị hành động tối ưu Q∗(s,a) nếu tất cả các cặp trạng thái-hành động được thử nghiệm đủ nhiều lần và tốc độ học α được giảm dần một cách thích hợp theo thời gian (ví dụ, thỏa mãn điều kiện Robbins-Monro).

Tối ưu (Tìm ra chính sách tối ưu): Có. Khi Q-Learning hội tụ đến Q∗(s,a), chính sách tối ưu π∗(s) có thể dễ dàng được suy ra bằng cách chọn hành động có giá trị Q lớn nhất tại mỗi trạng thái: π∗(s)=argmaxaQ∗(s,a).

Off-policy (Ngoài chính sách): Q-Learning là một thuật toán off-policy. Điều này có nghĩa là nó có thể học chính sách tối ưu Q∗(s,a) ngay cả khi các hành động được tạo ra từ một chính sách khác (ví dụ: chính sách ϵ-greedy dùng để khám phá). Nó học về chính sách tham lam argmaxaQ(s,a) trong khi vẫn tuân theo một chính sách khác để tạo ra hành vi.

Model-free (Không cần mô hình): Q-Learning không yêu cầu agent phải có mô hình của môi trường (tức là không cần biết hàm chuyển đổi trạng thái P(s′∣s,a) hay hàm phần thưởng R(s,a,s′)). Nó học trực tiếp từ kinh nghiệm tương tác với môi trường.

Độ phức tạp (ước lượng):

Thời gian: 
Mỗi bước cập nhật Q-value mất O(A) thời gian, trong đó A là số lượng hành động có thể có, để thực hiện thao tác maxa′Q(s′,a′).
Thời gian cần thiết để hội tụ có thể rất lớn, phụ thuộc vào kích thước của không gian trạng thái (S), không gian hành động (A), các tham số học (α,γ), và mức độ khám phá. Trong lý thuyết, nó có thể là đa thức theo S, A, và 1/(1−γ).

Không gian: O(S×A) để lưu trữ Q-table. Đây là một hạn chế lớn đối với các bài toán có không gian trạng thái hoặc không gian hành động lớn. Trong những trường hợp này, các kỹ thuật xấp xỉ hàm (function approximation), ví dụ như sử dụng mạng nơ-ron (Deep Q-Networks - DQN), thường được sử dụng thay cho Q-table tường minh.
![Q-Learning](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/Q-Learning.gif)
# 3. Kết luận
Dự án này đã thực hiện triển khai và mô tả một loạt các thuật toán tìm kiếm, từ cơ bản đến nâng cao, để giải quyết bài toán 8-puzzle. Các thuật toán bao gồm cả tìm kiếm không có thông tin, tìm kiếm có thông tin, các thuật toán tìm kiếm cục bộ và một số thuật toán cho các vấn đề phức tạp hơn như không gian trạng thái niềm tin.   

Một số kết quả đạt được:

Hiểu rõ về ý tưởng, cách hoạt động, ưu nhược điểm và độ phức tạp của nhiều thuật toán tìm kiếm phổ biến.

Có được một bộ mã nguồn Python triển khai các thuật toán này, có thể dùng để so sánh và đối chiếu hiệu suất của chúng trên bài toán 8-puzzle.

Phân tích được các đặc tính quan trọng như tính hoàn chỉnh, tính tối ưu, độ phức tạp thời gian và không gian của mỗi thuật toán trong bối cảnh giải quyết 8-puzzle.

Việc áp dụng các thuật toán này lên trò chơi 8 ô chữ cho thấy sự khác biệt rõ rệt về hiệu suất:

Các thuật toán có thông tin như A* và IDA* (với heuristic tốt) thường hiệu quả hơn nhiều trong việc tìm ra lời giải tối ưu so với các thuật toán không có thông tin.

Các thuật toán không có thông tin như BFS và IDS đảm bảo tính tối ưu về số bước nhưng có thể tốn kém về tài nguyên.

Các thuật toán tìm kiếm cục bộ nhanh và tiết kiệm bộ nhớ nhưng thường không đảm bảo tìm được lời giải hoặc lời giải tối ưu.

Nhìn chung, dự án cung cấp một cái nhìn toàn diện và thực tiễn về các phương pháp tìm kiếm trong trí tuệ nhân tạo thông qua một bài toán kinh điển.
