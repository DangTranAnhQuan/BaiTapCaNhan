# BaiTapCaNhan
Trang readme này trình bày mô tả chi tiết về các thuật toán tìm kiếm được triển khai trong mã nguồn Python để giải quyết bài toán 8-puzzle. Mỗi thuật toán được phân tích dựa trên ý tưởng cốt lõi, cơ chế hoạt động, các đặc điểm về tính hoàn chỉnh, tối ưu và độ phức tạp thuật toán ước lượng.

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
![IDS](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/IDS_new.gif)

# Thuật toán IDA*      
Ý tưởng cốt lõi: Kết hợp A* và IDS. Thực hiện tìm kiếm giới hạn "chi phí f" thay vì giới hạn độ sâu. Tối ưu và tiết kiệm bộ nhớ. 
Cách hoạt động: Đặt ngưỡng chi phí f. Thực hiện tìm kiếm kiểu DFS, cắt nhánh nếu f(n) > threshold. Nếu không tìm thấy, tăng ngưỡng và lặp lại. 
Đặc điểm: 
  Hoàn chỉnh: Có.
  Tối ưu: Có (với heuristic admissible).
Độ phức tạp (ước lượng): 
  Thời gian: O(b^d). Tương tự A* về số nút mở rộng.
  Không gian: O(bd). Rất tiết kiệm bộ nhớ.
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
![GreedySearch](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/GreedySearch_new.gif)

# Thuật toán BFS     
Ý tưởng cốt lõi: Khám phá trạng thái theo từng lớp độ sâu, đảm bảo tìm thấy đường đi ngắn nhất về số bước. 
Cách hoạt động: Sử dụng hàng đợi (Queue - FIFO). Thăm tất cả các nút ở độ sâu k trước khi thăm bất kỳ nút nào ở độ sâu k+1. Sử dụng tập visited để tránh duyệt lại trạng thái. 
Đặc điểm: 
  Hoàn chỉnh (Complete): Có.
  Tối ưu (Optimal): Có (về số bước đi).
Độ phức tạp (ước lượng): 
  Thời gian: O(b^d).
  Không gian: O(b^d).
![BFS](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/BFS_new.gif)

# Thuật toán Simple Hill Climbing  
Ý tưởng cốt lõi: Tìm kiếm cục bộ. Di chuyển đến trạng thái hàng xóm đầu tiên tốt hơn (heuristic h thấp hơn) trạng thái hiện tại. 
Cách hoạt động: Lặp: tìm hàng xóm, nếu có hàng xóm tốt hơn đầu tiên thì chuyển, nếu không thì dừng (kẹt). 
Đặc điểm: 
  Hoàn chỉnh: Không. Dễ bị kẹt ở cực trị cục bộ hoặc bình nguyên.
  Tối ưu: Không.
Độ phức tạp (ước lượng): 
  Thời gian: Nhanh cho mỗi bước, không đảm bảo thời gian tìm ra lời giải.
  Không gian: O(1).
![SimpleHill](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/SimpleHill_new.gif)

# Thuật toán Hill Climbing  
Ý tưởng cốt lõi: Tìm kiếm cục bộ. Xem xét tất cả hàng xóm và di chuyển đến hàng xóm tốt nhất (có h thấp nhất). 
Cách hoạt động: Lặp: đánh giá tất cả hàng xóm, chọn hàng xóm tốt nhất (nếu tốt hơn hiện tại) để di chuyển, nếu không thì dừng. 
Đặc điểm: Giống Simple Hill Climbing nhưng ít bị ảnh hưởng bởi thứ tự hàng xóm hơn. Vẫn bị kẹt. 
Độ phức tạp (ước lượng): 
  Thời gian: Mỗi bước tốn O(b), không đảm bảo thời gian tìm ra lời giải.
  Không gian: O(1).
![HillCLimbing](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/HillClimbing_1.gif)

# Thuật toán UCS          
Ý tưởng cốt lõi: Mở rộng nút có chi phí đường đi (g) thấp nhất tính từ nút bắt đầu. Tương đương BFS khi chi phí mọi hành động là 1. 
Cách hoạt động: Sử dụng hàng đợi ưu tiên (Priority Queue) sắp xếp theo chi phí g. Luôn chọn nút có g nhỏ nhất để mở rộng. Sử dụng tập visited. 
Đặc điểm: 
  Hoàn chỉnh: Có.
  Tối ưu: Có (tìm đường đi với tổng chi phí thấp nhất).
Độ phức tạp (ước lượng): (Với chi phí bước là 1, C* = d) 
  Thời gian: O(b^d) hoặc O(b^(1 + floor(C*/epsilon))). Tương tự BFS trong trường hợp này. Có thể lên đến O(V log V) nếu xét chi phí hàng đợi ưu tiên.
  Không gian: O(b^d).
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
![DFS](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/DFS_new.gif)

# Thuật toán Stochastic Climbing
Ý tưởng cốt lõi: Tìm kiếm cục bộ. Chọn ngẫu nhiên một hàng xóm, nếu tốt hơn thì di chuyển, nếu không thì thử hàng xóm ngẫu nhiên khác. 
Cách hoạt động: Lặp: chọn ngẫu nhiên hàng xóm, nếu tốt hơn thì chuyển, nếu không thì bỏ qua và thử lại. Dừng nếu hết lựa chọn tốt hơn. 
Đặc điểm: Không hoàn chỉnh, không tối ưu. Tính ngẫu nhiên có thể giúp khám phá, nhưng vẫn có thể bị kẹt. 
Độ phức tạp (ước lượng): 
  Thời gian: Không xác định, phụ thuộc may mắn.
  Không gian: O(1).
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
![BeamSearch](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/BeamSearch.gif)

# Thuật toán Simulated Annealing
Ý tưởng cốt lõi: Thuật toán tối ưu hóa xác suất. Cho phép di chuyển đến trạng thái tệ hơn với xác suất giảm dần theo "nhiệt độ" T, giúp thoát khỏi cực trị cục bộ. 
Cách hoạt động: Lặp: chọn hàng xóm ngẫu nhiên. Nếu tốt hơn, di chuyển. Nếu tệ hơn, di chuyển với xác suất exp(delta_E / T). Giảm T theo thời gian. 
Đặc điểm: Có khả năng tìm ra giải pháp tốt nếu làm nguội đủ chậm, nhưng không đảm bảo. 
Độ phức tạp (ước lượng): 
Thời gian: Phụ thuộc vào lịch trình làm nguội.
Không gian: O(1).
![SimulatedAnnealing](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/SimulatedAnnealing.gif)

# Thuật toán Genetic Algorithm
Ý tưởng cốt lõi: Mô phỏng tiến hóa. Duy trì quần thể trạng thái, áp dụng chọn lọc, lai ghép, đột biến qua các thế hệ để tìm trạng thái tốt nhất (fitness cao - h thấp). 
Cách hoạt động: Tối ưu hóa trạng thái, không phải đường đi. Lặp qua các thế hệ, đánh giá fitness, chọn lọc, lai ghép (crossover), đột biến (mutate) để tạo thế hệ mới. 
Đặc điểm: Metaheuristic, không đảm bảo hoàn chỉnh/tối ưu. Tìm trạng thái đích nhưng không trả về đường đi. 
Độ phức tạp (ước lượng): 
  Thời gian: O(generations * population_size * cost_per_individual).
  Không gian: O(population_size).
![GeneticAlgorithm](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/GeneticAlgorithm.gif)

# Thuật toán And Or Search 
Ý tưởng cốt lõi: Dùng cho bài toán phân rã thành bài toán con (AND nodes, OR nodes). 
Cách hoạt động: Thực chất là DFS/Backtracking với giới hạn độ sâu và kiểm tra chu trình cục bộ. Không thể hiện cấu trúc AND-OR thực sự cho 8-puzzle. 
Đặc điểm: Giống DFS có giới hạn độ sâu. 
Độ phức tạp (ước lượng): 
  Thời gian: O(b^M) với M là max_depth.
  Không gian: O(bM) cho ngăn xếp đệ quy.
![AndOrSearch](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/AndOrSearch.gif)

# Thuật toán Belief Search
Thuật toán Belief Search
Ý tưởng cốt lõi: Tìm kiếm trong không gian các "trạng thái niềm tin" (tập hợp các trạng thái vật lý có thể) khi trạng thái hiện tại không chắc chắn. 
Cách hoạt động: Thực hiện BFS trên không gian trạng thái niềm tin. Một hành động biến đổi toàn bộ tập trạng thái hiện tại thành tập trạng thái mới. Đích đạt được khi mọi trạng thái trong tập đều là đích. 
Đặc điểm: Giải quyết bài toán trong môi trường không chắc chắn/quan sát một phần. 
Độ phức tạp (ước lượng): 
  Thời gian: Có thể là O(2^V). Rất cao.
  Không gian: Có thể là O(2^V).
![BeliefSearch](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/BeliefSearch.gif)

# Thuật toán Partial Observations
Ý tưởng cốt lõi: Tương tự belief_search, xử lý việc không biết chắc trạng thái hiện tại.
Cách hoạt động: Giống belief_search, duy trì và cập nhật tập hợp các trạng thái có thể (belief state). Có thêm giới hạn max_steps.
Đặc điểm: Giống belief_search.
Độ phức tạp (ước lượng): 
  Thời gian: Lũy thừa theo V, có thể lên tới O(2^V).
  Không gian: Lũy thừa theo V, có thể lên tới O(2^V).
![PartialObservations](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/PartialObservations_new.gif)

# Thuật toán Backtracking Search
Ý tưởng cốt lõi: Xây dựng giải pháp từng bước, nếu gặp ngõ cụt, quay lại bước trước và thử lựa chọn khác. 
Cách hoạt động: Sử dụng đệ quy để khám phá đường đi. Thêm trạng thái mới nếu hợp lệ và chưa có trong đường đi hiện tại. Nếu đạt đích, trả về. Nếu hết lựa chọn hoặc đạt giới hạn độ sâu, quay lui. 
Đặc điểm: Là một dạng DFS. 
  Hoàn chỉnh: Có (nếu max_depth đủ lớn).
  Tối ưu: Không.
Độ phức tạp (ước lượng): 
  Thời gian: O(b^M) với M là max_depth. Tương tự DFS.
  Không gian: O(M). Chỉ cần lưu đường đi hiện tại.
![BacktrackingSearch](https://github.com/DangTranAnhQuan/BaiTapCaNhan/blob/main/BacktrackingSearch.gif)
