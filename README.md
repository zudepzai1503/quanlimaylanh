<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Trình Theo Dõi Bảo Trì Máy Lạnh Khách Sạn</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Thư viện cho bảng chọn ngày tháng -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/flatpickr/dist/flatpickr.min.css">
    <script src="https://cdn.jsdelivr.net/npm/flatpickr"></script>
    <script src="https://npmcdn.com/flatpickr/dist/l10n/vn.js"></script> <!-- Gói ngôn ngữ Tiếng Việt -->

    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f0f4f8; }
        .card { background-color: white; border-radius: 12px; box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1); padding: 24px; margin-bottom: 24px; }
        .table-header { background-color: #e2e8f0; }
        .table-header th { padding: 12px 16px; font-weight: 600; color: #2d3748; }
        .table-row td { padding: 12px 16px; border-bottom: 1px solid #e2e8f0; }
        .btn { transition: all 0.2s ease-in-out; }
        .btn:hover { transform: translateY(-1px); box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
        .modal-backdrop { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background-color: rgba(0,0,0,0.5); display: flex; justify-content: center; align-items: center; z-index: 50; }
        .modal-content { background-color: white; padding: 30px; border-radius: 12px; width: 90%; max-width: 700px; max-height: 90vh; overflow-y: auto; }
        .month-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 16px; }
        .month-card { border: 1px solid #e2e8f0; border-radius: 8px; padding: 16px; cursor: pointer; transition: all 0.2s ease-in-out; }
        .month-card:hover { border-color: #a5b4fc; background-color: #f7fafc; transform: translateY(-2px); }
        .flatpickr-calendar { font-family: 'Inter', sans-serif; border-radius: 8px; box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1); }
        .flatpickr-month { padding-bottom: 8px; }
        .flatpickr-current-month .flatpickr-monthDropdown-months, input.flatpickr-current-month { font-size: 1rem; font-weight: 600; }
        .detail-row { cursor: pointer; }
        .detail-row:hover { background-color: #f9fafb; }
        .log-container { max-height: 320px; overflow-y: auto; }
        .stat-card { text-align: center; padding: 20px; border-radius: 12px; }
        .stat-card .icon { font-size: 2.5rem; margin-bottom: 1rem; }
        .stat-card .title { font-size: 1rem; color: #4a5568; margin-bottom: 0.5rem; }
        .stat-card .value { font-size: 2rem; font-weight: 700; }
    </style>
</head>
<body class="p-4 md:p-8">

    <div class="max-w-7xl mx-auto">
        <header class="text-center mb-8">
            <h1 class="text-3xl md:text-4xl font-bold text-gray-800">Hệ Thống Quản Lý Bảo Trì Máy Lạnh</h1>
            <p class="text-gray-600 mt-2">Dễ dàng theo dõi, quản lý và lên lịch vệ sinh, sửa chữa cho khách sạn của bạn.</p>
        </header>

        <!-- Form nhập liệu -->
        <div class="card" id="form-card">
             <h2 class="text-2xl font-bold text-gray-700 mb-6">Thêm Lượt Bảo Trì Mới</h2>
            <form id="maintenance-form" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <div>
                    <label for="room-number" class="block text-sm font-medium text-gray-700 mb-1">Chọn Phòng</label>
                    <select id="room-number" required class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500">
                        <option value="Sảnh lễ tân">Sảnh lễ tân</option>
                        <option value="P.101">Phòng 101</option> <option value="P.102">Phòng 102</option>
                        <option value="P.201">Phòng 201</option> <option value="P.202">Phòng 202</option>
                        <option value="P.301">Phòng 301</option> <option value="P.302">Phòng 302</option>
                        <option value="P.401">Phòng 401</option> <option value="P.402">Phòng 402</option>
                        <option value="P.501">Phòng 501</option> <option value="P.502">Phòng 502</option>
                    </select>
                </div>
                <div>
                    <label for="maintenance-date" class="block text-sm font-medium text-gray-700 mb-1">Ngày Thực Hiện</label>
                    <input type="text" id="maintenance-date" required class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500" placeholder="Chọn ngày...">
                </div>
                <div>
                    <label for="service-type" class="block text-sm font-medium text-gray-700 mb-1">Hạng Mục</label>
                    <select id="service-type" required class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500">
                        <option value="Vệ sinh">Vệ sinh định kỳ</option>
                        <option value="Sửa chữa">Sửa chữa</option>
                    </select>
                </div>
                <div>
                    <label for="cost" class="block text-sm font-medium text-gray-700 mb-1">Chi Phí (VNĐ)</label>
                    <input type="text" inputmode="numeric" id="cost" placeholder="Ví dụ: 300000" required class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500" autocomplete="off">
                </div>
                <div class="md:col-span-2">
                    <label for="notes" class="block text-sm font-medium text-gray-700 mb-1">Ghi Chú</label>
                    <input type="text" id="notes" placeholder="Ví dụ: Thay gas, sửa quạt dàn nóng..." class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500" autocomplete="off">
                </div>
                <div class="md:col-span-2 lg:col-span-3 flex justify-end">
                    <button type="submit" class="btn bg-indigo-600 text-white font-bold py-2 px-6 rounded-lg shadow-md hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        <i class="fas fa-plus mr-2"></i>Thêm vào sổ
                    </button>
                </div>
            </form>
        </div>

        <!-- Bảng lịch sử bảo trì -->
        <div class="card">
            <div class="flex flex-col md:flex-row justify-between items-center mb-4">
                <h2 class="text-2xl font-bold text-gray-700">Nhật Ký Bảo Trì</h2>
                <div class="flex items-center gap-2 md:gap-4 mt-4 md:mt-0">
                    <button id="prev-log-month-btn" class="p-2 rounded-full hover:bg-gray-200 transition" aria-label="Tháng trước"><i class="fas fa-chevron-left"></i></button>
                    <span id="displayed-log-month" class="text-xl font-bold text-gray-700 w-24 text-center"></span>
                    <button id="next-log-month-btn" class="p-2 rounded-full hover:bg-gray-200 transition" aria-label="Tháng sau"><i class="fas fa-chevron-right"></i></button>
                </div>
            </div>
            <div class="log-container">
                <table class="w-full text-sm text-left text-gray-500">
                    <thead class="text-xs text-gray-700 uppercase table-header sticky top-0">
                        <tr>
                            <th scope="col" class="px-6 py-3">Phòng/Khu vực</th>
                            <th scope="col" class="px-6 py-3">Ngày</th>
                            <th scope="col" class="px-6 py-3">Hạng Mục</th>
                            <th scope="col" class="px-6 py-3">Chi Phí</th>
                            <th scope="col" class="px-6 py-3">Ghi Chú</th>
                            <th scope="col" class="px-6 py-3">Thao Tác</th>
                        </tr>
                    </thead>
                    <tbody id="maintenance-log"></tbody>
                </table>
                 <p id="no-data-message" class="text-center text-gray-500 py-8">Chưa có dữ liệu bảo trì nào.</p>
            </div>
        </div>
        
        <!-- Lịch Hoạt Động Theo Tháng -->
        <div class="card">
            <div class="flex flex-col md:flex-row justify-between items-center mb-6">
                <h2 class="text-2xl font-bold text-gray-700">Lịch Hoạt Động Bảo Trì</h2>
                <div class="flex items-center gap-2 md:gap-4 mt-4 md:mt-0">
                    <button id="prev-year-btn" class="p-2 rounded-full hover:bg-gray-200 transition" aria-label="Năm trước"><i class="fas fa-chevron-left"></i></button>
                    <span id="displayed-year" class="text-xl font-bold text-gray-700 w-20 text-center"></span>
                    <button id="next-year-btn" class="p-2 rounded-full hover:bg-gray-200 transition" aria-label="Năm sau"><i class="fas fa-chevron-right"></i></button>
                </div>
            </div>
            <div id="monthly-schedule" class="month-grid"></div>
        </div>

        <footer class="text-center mt-8 mb-4 space-y-6">
            <button id="open-report-btn" class="btn bg-green-600 text-white font-bold py-3 px-6 rounded-lg shadow-md hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500 inline-block">
                <i class="fas fa-chart-pie mr-2"></i>Xem Báo Cáo Tổng
            </button>
            <div class="pt-6 border-t border-gray-200">
                 <h3 class="text-lg font-semibold text-gray-700 mb-3">Quản lý Dữ liệu</h3>
                <div class="flex justify-center items-center gap-2 md:gap-4 flex-wrap">
                    <button id="open-export-month-btn" class="btn bg-cyan-600 text-white font-bold py-2 px-5 rounded-lg hover:bg-cyan-700">
                       <i class="fas fa-calendar-alt mr-2"></i>Xuất Báo Cáo Tháng
                    </button>
                    <button id="export-data-btn" class="btn bg-blue-600 text-white font-bold py-2 px-5 rounded-lg hover:bg-blue-700">
                        <i class="fas fa-download mr-2"></i>Sao lưu Toàn Bộ
                    </button>
                    <button id="import-data-btn" class="btn bg-orange-500 text-white font-bold py-2 px-5 rounded-lg hover:bg-orange-600">
                        <i class="fas fa-upload mr-2"></i>Phục hồi Dữ Liệu
                    </button>
                </div>
                <input type="file" id="import-file-input" class="hidden" accept=".json, .txt">
            </div>
        </footer>
    </div>

     <!-- Modal Xác nhận Nhập Dữ Liệu -->
    <div id="import-confirm-modal" class="modal-backdrop hidden">
        <div class="modal-content">
            <h3 class="text-xl font-bold text-red-600 mb-4 flex items-center"><i class="fas fa-exclamation-triangle mr-3"></i>Cảnh báo!</h3>
            <p class="text-gray-600 mb-2">Bạn có chắc chắn muốn nhập dữ liệu mới không? Hành động này sẽ <strong class="font-bold text-red-700">ghi đè toàn bộ dữ liệu hiện tại</strong> và không thể hoàn tác.</p>
            <p class="text-gray-600 mb-6">Dữ liệu sẽ được nạp từ tệp: <strong id="import-filename" class="font-mono bg-gray-100 px-1 rounded"></strong></p>
            <div class="flex justify-end gap-4">
                <button id="cancel-import-btn" class="btn bg-gray-300 text-gray-800 font-bold py-2 px-4 rounded-lg hover:bg-gray-400">Hủy</button>
                <button id="confirm-import-btn" class="btn bg-red-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-red-700">Tôi hiểu, Xác nhận Nhập</button>
            </div>
        </div>
    </div>


    <!-- Modal Báo Cáo & Thống Kê -->
    <div id="report-modal" class="modal-backdrop hidden">
        <div class="modal-content">
            <h3 class="text-3xl font-bold text-gray-800 mb-8 text-center">Báo Cáo Toàn Bộ & Thống Kê</h3>
            <div class="space-y-8">
                 <!-- Tổng quan chi phí -->
                <div>
                    <h4 class="text-xl font-bold text-gray-700 mb-4">Tổng Quan Chi Phí</h4>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="stat-card bg-blue-100 text-blue-800">
                            <div class="icon"><i class="fas fa-soap"></i></div>
                            <div class="title">Tổng Chi Phí Vệ Sinh</div>
                            <div id="total-cleaning-cost" class="value">0 VNĐ</div>
                        </div>
                        <div class="stat-card bg-yellow-100 text-yellow-800">
                            <div class="icon"><i class="fas fa-tools"></i></div>
                            <div class="title">Tổng Chi Phí Sửa Chữa</div>
                            <div id="total-repair-cost" class="value">0 VNĐ</div>
                        </div>
                    </div>
                </div>

                <!-- Thống kê theo phòng -->
                <div>
                    <h4 class="text-xl font-bold text-gray-700 mb-4">Thống Kê Tần Suất Theo Phòng</h4>
                    <div class="overflow-x-auto max-h-[40vh]">
                        <table class="w-full text-sm text-left text-gray-500">
                            <thead class="text-xs text-gray-700 uppercase table-header sticky top-0">
                                <tr>
                                    <th scope="col" class="px-6 py-3 rounded-l-lg">Phòng/Khu vực</th>
                                    <th scope="col" class="px-6 py-3 text-center">Số Lần Vệ Sinh</th>
                                    <th scope="col" class="px-6 py-3 text-center rounded-r-lg">Số Lần Sửa Chữa</th>
                                </tr>
                            </thead>
                            <tbody id="room-stats-body"></tbody>
                        </table>
                    </div>
                </div>
            </div>
            <div class="flex justify-end mt-8">
                 <button id="close-report-btn" class="btn bg-gray-200 text-gray-800 font-bold py-2 px-4 rounded-lg hover:bg-gray-300">Đóng</button>
            </div>
        </div>
    </div>
    
    <!-- Modal Chọn Tháng để Xuất CSV -->
    <div id="export-month-modal" class="modal-backdrop hidden">
        <div class="modal-content max-w-md">
            <h3 class="text-2xl font-bold text-gray-800 mb-6">Chọn tháng để xuất báo cáo</h3>
            <div class="flex items-center gap-4">
                <div class="flex-1">
                    <label for="export-select-month" class="block text-sm font-medium text-gray-700">Tháng</label>
                    <select id="export-select-month" class="mt-1 w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500">
                    </select>
                </div>
                <div class="flex-1">
                    <label for="export-select-year" class="block text-sm font-medium text-gray-700">Năm</label>
                    <select id="export-select-year" class="mt-1 w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500">
                    </select>
                </div>
            </div>
            <div class="flex justify-end gap-4 mt-8">
                <button id="cancel-export-month-btn" class="btn bg-gray-300 text-gray-800 font-bold py-2 px-4 rounded-lg hover:bg-gray-400">Hủy</button>
                <button id="confirm-export-month-btn" class="btn bg-teal-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-teal-700">
                    <i class="fas fa-file-csv mr-2"></i>Xuất tệp
                </button>
            </div>
        </div>
    </div>


    <!-- Modal Chi tiết Tháng -->
    <div id="month-details-modal" class="modal-backdrop hidden">
        <div class="modal-content">
             <h3 id="month-details-title" class="text-2xl font-bold text-gray-800 mb-6"></h3>
             <div class="overflow-y-auto max-h-[60vh]">
                  <table class="w-full text-sm text-left">
                      <thead class="text-xs text-gray-700 uppercase bg-gray-100 sticky top-0">
                          <tr>
                              <th class="px-4 py-3">Ngày</th>
                              <th class="px-4 py-3">Phòng</th>
                              <th class="px-4 py-3">Hạng mục</th>
                              <th class="px-4 py-3">Ghi chú</th>
                          </tr>
                      </thead>
                      <tbody id="month-details-body"></tbody>
                  </table>
             </div>
             <div class="flex justify-end mt-6">
                 <button id="close-month-details-btn" class="btn bg-gray-200 text-gray-800 font-bold py-2 px-4 rounded-lg hover:bg-gray-300">Đóng</button>
             </div>
        </div>
    </div>

    <!-- Modal Chi tiết & Chỉnh sửa Record -->
    <div id="record-modal" class="modal-backdrop hidden">
        <div class="modal-content">
            <h3 class="text-2xl font-bold text-gray-800 mb-6">Chi Tiết Bảo Trì</h3>
            <form id="edit-form" class="space-y-4">
                 <div>
                    <label for="edit-room-number" class="block text-sm font-medium text-gray-700">Phòng/Khu vực</label>
                    <select id="edit-room-number" required class="mt-1 w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500"></select>
                </div>
                <div>
                    <label for="edit-maintenance-date" class="block text-sm font-medium text-gray-700">Ngày Thực Hiện</label>
                    <input type="text" id="edit-maintenance-date" required class="mt-1 w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500">
                </div>
                <div>
                    <label for="edit-service-type" class="block text-sm font-medium text-gray-700">Hạng Mục</label>
                    <select id="edit-service-type" required class="mt-1 w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500"></select>
                </div>
                 <div>
                    <label for="edit-cost" class="block text-sm font-medium text-gray-700">Chi Phí (VNĐ)</label>
                    <input type="text" inputmode="numeric" id="edit-cost" required class="mt-1 w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500" autocomplete="off">
                </div>
                 <div>
                    <label for="edit-notes" class="block text-sm font-medium text-gray-700">Ghi Chú</label>
                    <input type="text" id="edit-notes" class="mt-1 w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500" autocomplete="off">
                </div>
            </form>
            <div class="flex justify-between items-center mt-8">
                <button id="delete-record-btn" class="btn text-red-600 font-bold py-2 px-4 rounded-lg hover:bg-red-50"><i class="fas fa-trash-alt mr-2"></i>Xóa</button>
                <div class="flex gap-4">
                     <button id="cancel-edit-btn" class="btn bg-gray-200 text-gray-800 font-bold py-2 px-4 rounded-lg hover:bg-gray-300">Hủy</button>
                     <button id="save-edit-btn" class="btn bg-indigo-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-indigo-700">Lưu thay đổi</button>
                </div>
            </div>
        </div>
    </div>

<script>
document.addEventListener('DOMContentLoaded', () => {
    // DOM Elements
    const form = document.getElementById('maintenance-form');
    const logBody = document.getElementById('maintenance-log');
    const noDataMessage = document.getElementById('no-data-message');
    const monthlyScheduleEl = document.getElementById('monthly-schedule');
    const costInput = document.getElementById('cost');
    const prevYearBtn = document.getElementById('prev-year-btn');
    const nextYearBtn = document.getElementById('next-year-btn');
    const yearDisplayEl = document.getElementById('displayed-year');
    const prevLogMonthBtn = document.getElementById('prev-log-month-btn');
    const nextLogMonthBtn = document.getElementById('next-log-month-btn');
    const displayedLogMonthEl = document.getElementById('displayed-log-month');
    
    // Modal Elements
    const monthDetailsModal = document.getElementById('month-details-modal');
    const monthDetailsTitle = document.getElementById('month-details-title');
    const monthDetailsBody = document.getElementById('month-details-body');
    const closeMonthDetailsBtn = document.getElementById('close-month-details-btn');

    const recordModal = document.getElementById('record-modal');
    const editForm = document.getElementById('edit-form');
    const cancelEditBtn = document.getElementById('cancel-edit-btn');
    const saveEditBtn = document.getElementById('save-edit-btn');
    const deleteRecordBtn = document.getElementById('delete-record-btn');

    const reportModal = document.getElementById('report-modal');
    const openReportBtn = document.getElementById('open-report-btn');
    const closeReportBtn = document.getElementById('close-report-btn');
    const totalCleaningCostEl = document.getElementById('total-cleaning-cost');
    const totalRepairCostEl = document.getElementById('total-repair-cost');
    const roomStatsBody = document.getElementById('room-stats-body');
    
    const importConfirmModal = document.getElementById('import-confirm-modal');
    const cancelImportBtn = document.getElementById('cancel-import-btn');
    const confirmImportBtn = document.getElementById('confirm-import-btn');
    const importFilenameEl = document.getElementById('import-filename');
    
    const exportDataBtn = document.getElementById('export-data-btn');
    const openExportMonthBtn = document.getElementById('open-export-month-btn');
    const importDataBtn = document.getElementById('import-data-btn');
    const importFileInput = document.getElementById('import-file-input');

    const exportMonthModal = document.getElementById('export-month-modal');
    const exportSelectMonth = document.getElementById('export-select-month');
    const exportSelectYear = document.getElementById('export-select-year');
    const cancelExportMonthBtn = document.getElementById('cancel-export-month-btn');
    const confirmExportMonthBtn = document.getElementById('confirm-export-month-btn');

    // State
    let maintenanceRecords = JSON.parse(localStorage.getItem('maintenanceRecords')) || [];
    let displayedLogDate = new Date();
    let displayedScheduleYear = new Date().getFullYear();
    let activeRecordId = null;
    let editFlatpickrInstance = null;
    let dataToImport = null;
    const roomOptions = Array.from(document.getElementById('room-number').options).map(o => o.value);
    const serviceOptions = Array.from(document.getElementById('service-type').options).map(o => o.value);

    // --- HELPER FUNCTIONS ---
    const formatCurrency = (amount) => {
        if (isNaN(amount)) return '0 VNĐ';
        const formattedNumber = Number(amount).toLocaleString('en-US');
        return `${formattedNumber} VNĐ`;
    };
    const formatDate = (dateInput) => {
        if (!dateInput) return '';
        let date = (dateInput instanceof Date) ? dateInput : new Date(dateInput.replace(/-/g, '/'));
        if (isNaN(date.getTime())) return '';
        return `${String(date.getDate()).padStart(2, '0')}/${String(date.getMonth() + 1).padStart(2, '0')}/${date.getFullYear()}`;
    };
    const saveToLocalStorage = () => localStorage.setItem('maintenanceRecords', JSON.stringify(maintenanceRecords));
    
    // --- RENDER FUNCTIONS ---
    const renderAll = () => {
        renderLog();
        renderMonthlySchedule();
    }

    const renderLog = () => {
        logBody.innerHTML = '';
        const year = displayedLogDate.getFullYear();
        const month = displayedLogDate.getMonth();
        
        displayedLogMonthEl.textContent = `${month + 1}/${year}`;

        const filteredRecords = maintenanceRecords
            .filter(r => {
                const d = new Date(r.date);
                return d.getFullYear() === year && d.getMonth() === month;
            })
            .sort((a, b) => new Date(a.date) - new Date(b.date));
        
        noDataMessage.classList.toggle('hidden', filteredRecords.length > 0);
        
        filteredRecords.forEach(record => {
            const row = document.createElement('tr');
            row.className = 'bg-white border-b';
            row.innerHTML = `
                <td class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap">${record.room}</td>
                <td class="px-6 py-4">${formatDate(record.date)}</td>
                <td class="px-6 py-4"><span class="px-2 py-1 font-semibold leading-tight rounded-full ${record.service === 'Vệ sinh' ? 'bg-blue-100 text-blue-800' : 'bg-yellow-100 text-yellow-800'}">${record.service}</span></td>
                <td class="px-6 py-4">${formatCurrency(record.cost)}</td>
                <td class="px-6 py-4">${record.notes || 'N/A'}</td>
                <td class="px-6 py-4"><button class="text-indigo-500 hover:text-indigo-700 edit-btn" data-id="${record.id}" aria-label="Sửa"><i class="fas fa-pencil-alt"></i></button></td>
            `;
            logBody.appendChild(row);
        });
    };
    
    const renderMonthlySchedule = () => {
        monthlyScheduleEl.innerHTML = '';
        yearDisplayEl.textContent = displayedScheduleYear;
        const monthNames = ["Tháng 1", "Tháng 2", "Tháng 3", "Tháng 4", "Tháng 5", "Tháng 6", "Tháng 7", "Tháng 8", "Tháng 9", "Tháng 10", "Tháng 11", "Tháng 12"];
        const recordsByMonth = Array.from({ length: 12 }, () => []);
        
        maintenanceRecords
            .filter(r => new Date(r.date).getFullYear() === displayedScheduleYear)
            .forEach(r => recordsByMonth[new Date(r.date).getMonth()].push(r));

        recordsByMonth.forEach((records, i) => {
            const monthCard = document.createElement('div');
            monthCard.className = `month-card ${records.length === 0 ? 'opacity-60' : ''}`;
            monthCard.dataset.month = i;
            monthCard.dataset.year = displayedScheduleYear;
            monthCard.innerHTML = `
                <h3 class="text-lg font-bold text-indigo-700 mb-2">${monthNames[i]}</h3>
                <div class="text-center text-gray-600">
                    <span class="text-2xl font-bold">${records.length}</span>
                    <span class="text-sm block">hoạt động</span>
                </div>
            `;
            monthlyScheduleEl.appendChild(monthCard);
        });
    };

    const renderReport = () => {
        let totalCleaningCost = 0;
        let totalRepairCost = 0;
        const roomStats = {};

        roomOptions.forEach(room => {
            roomStats[room] = { cleaning: 0, repair: 0 };
        });

        maintenanceRecords.forEach(record => {
            if (record.service === 'Vệ sinh') {
                totalCleaningCost += Number(record.cost);
                if (roomStats[record.room]) roomStats[record.room].cleaning++;
            } else if (record.service === 'Sửa chữa') {
                totalRepairCost += Number(record.cost);
                 if (roomStats[record.room]) roomStats[record.room].repair++;
            }
        });

        totalCleaningCostEl.textContent = formatCurrency(totalCleaningCost);
        totalRepairCostEl.textContent = formatCurrency(totalRepairCost);

        roomStatsBody.innerHTML = '';
        roomOptions.forEach(room => {
            const stats = roomStats[room];
            const row = document.createElement('tr');
            row.className = 'bg-white border-b table-row';
            row.innerHTML = `
                <td class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap">${room}</td>
                <td class="px-6 py-4 text-center text-lg font-bold text-blue-600">${stats.cleaning}</td>
                <td class="px-6 py-4 text-center text-lg font-bold text-yellow-600">${stats.repair}</td>
            `;
            roomStatsBody.appendChild(row);
        });
    };

    // --- MODAL FUNCTIONS ---
    const openMonthDetailsModal = (month, year) => {
        monthDetailsTitle.textContent = `Chi tiết tháng ${parseInt(month) + 1}/${year}`;
        monthDetailsBody.innerHTML = '';
        const records = maintenanceRecords
            .filter(r => {
                const d = new Date(r.date);
                return d.getMonth() == month && d.getFullYear() == year;
            })
            .sort((a,b) => new Date(a.date) - new Date(b.date));
        
        if (records.length === 0) {
            monthDetailsBody.innerHTML = '<tr><td colspan="4" class="text-center py-8 text-gray-500">Không có hoạt động nào trong tháng này.</td></tr>';
        } else {
            records.forEach(record => {
                const row = document.createElement('tr');
                row.className = 'detail-row border-b';
                row.dataset.id = record.id;
                row.innerHTML = `
                    <td class="px-4 py-3 font-semibold">${new Date(record.date).getDate()}</td>
                    <td class="px-4 py-3">${record.room}</td>
                    <td class="px-4 py-3"><span class="px-2 py-1 text-xs font-semibold leading-tight rounded-full ${record.service === 'Vệ sinh' ? 'bg-blue-100 text-blue-800' : 'bg-yellow-100 text-yellow-800'}">${record.service}</span></td>
                    <td class="px-4 py-3 truncate" title="${record.notes || ''}">${record.notes || 'N/A'}</td>
                `;
                monthDetailsBody.appendChild(row);
            });
        }
        monthDetailsModal.classList.remove('hidden');
    };

    const closeMonthDetailsModal = () => monthDetailsModal.classList.add('hidden');

    const openRecordModal = (recordId) => {
        const record = maintenanceRecords.find(r => r.id === recordId);
        if (!record) return;

        activeRecordId = recordId;
        const fillSelect = (selectId, options, selected) => {
            const select = document.getElementById(selectId);
            select.innerHTML = options.map(opt => `<option value="${opt}" ${opt === selected ? 'selected' : ''}>${opt}</option>`).join('');
        };

        fillSelect('edit-room-number', roomOptions, record.room);
        fillSelect('edit-service-type', serviceOptions, record.service);
        document.getElementById('edit-cost').value = record.cost;
        document.getElementById('edit-notes').value = record.notes;
        
        if (!editFlatpickrInstance) {
            editFlatpickrInstance = flatpickr("#edit-maintenance-date", { locale: "vn", altInput: true, altFormat: "d/m/Y", dateFormat: "Y-m-d" });
        }
        editFlatpickrInstance.setDate(record.date, true);
        recordModal.classList.remove('hidden');
    };

    const closeRecordModal = () => {
        recordModal.classList.add('hidden');
        activeRecordId = null;
        editForm.reset();
    };

    const openReportModal = () => {
        renderReport();
        reportModal.classList.remove('hidden');
    };

    const closeReportModal = () => reportModal.classList.add('hidden');

    const openExportMonthModal = () => {
        // Populate years
        const years = [...new Set(maintenanceRecords.map(r => new Date(r.date).getFullYear()))].sort((a,b) => b-a);
        exportSelectYear.innerHTML = years.map(y => `<option value="${y}">${y}</option>`).join('');
        
        // Populate months
        exportSelectMonth.innerHTML = Array.from({length: 12}, (_, i) => `<option value="${i}">${i + 1}</option>`).join('');
        
        // Set default to current month/year
        const now = new Date();
        exportSelectMonth.value = now.getMonth();
        exportSelectYear.value = now.getFullYear();

        exportMonthModal.classList.remove('hidden');
    };

    const closeExportMonthModal = () => exportMonthModal.classList.add('hidden');

    const exportMonthToCsv = (month, year) => {
        const recordsToExport = maintenanceRecords.filter(r => {
            const d = new Date(r.date);
            return d.getMonth() == month && d.getFullYear() == year;
        });

        if (recordsToExport.length === 0) {
            alert(`Không có dữ liệu để xuất cho Tháng ${parseInt(month) + 1}/${year}.`);
            return;
        }
        let totalCleaningCost = 0;
        let totalRepairCost = 0;
        recordsToExport.forEach(r => {
            if (r.service === 'Vệ sinh') totalCleaningCost += Number(r.cost);
            if (r.service === 'Sửa chữa') totalRepairCost += Number(r.cost);
        });

        let csvContent = "data:text/csv;charset=utf-8,\uFEFF";
        csvContent += `Báo cáo chi tiết Tháng ${parseInt(month) + 1}/${year}\n\n`;
        csvContent += "Tổng quan chi phí\n";
        csvContent += "Hạng mục,Tổng Chi Phí\n";
        csvContent += `Vệ sinh,${totalCleaningCost}\n`;
        csvContent += `Sửa chữa,${totalRepairCost}\n\n`;
        csvContent += "Danh sách chi tiết\n";
        csvContent += "Ngày,Phòng,Hạng Mục,Chi Phí,Ghi Chú\n";

        recordsToExport.sort((a,b) => new Date(a.date) - new Date(b.date));
        recordsToExport.forEach(record => {
            const row = [formatDate(record.date), record.room, record.service, record.cost, `"${record.notes.replace(/"/g, '""')}"`].join(",");
            csvContent += row + "\r\n";
        });
        
        const encodedUri = encodeURI(csvContent);
        const a = document.createElement('a');
        a.href = encodedUri;
        a.download = `baocao_thang_${parseInt(month) + 1}-${year}.csv`;
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
    }

    // --- EVENT LISTENERS ---
    prevYearBtn.addEventListener('click', () => { displayedScheduleYear--; renderMonthlySchedule(); });
    nextYearBtn.addEventListener('click', () => { displayedScheduleYear++; renderMonthlySchedule(); });

    prevLogMonthBtn.addEventListener('click', () => {
        displayedLogDate.setMonth(displayedLogDate.getMonth() - 1);
        renderLog();
    });
    nextLogMonthBtn.addEventListener('click', () => {
        displayedLogDate.setMonth(displayedLogDate.getMonth() + 1);
        renderLog();
    });

    monthlyScheduleEl.addEventListener('click', (e) => {
        const monthCard = e.target.closest('.month-card');
        if (monthCard) openMonthDetailsModal(monthCard.dataset.month, monthCard.dataset.year);
    });
    
    monthDetailsBody.addEventListener('click', (e) => {
        const detailRow = e.target.closest('.detail-row');
        if (detailRow) openRecordModal(detailRow.dataset.id);
    });

    logBody.addEventListener('click', (e) => {
        const editButton = e.target.closest('.edit-btn');
        if (editButton) openRecordModal(editButton.dataset.id);
    });

    form.addEventListener('submit', (e) => {
        e.preventDefault();
        maintenanceRecords.push({
            id: Date.now().toString(),
            room: document.getElementById('room-number').value,
            date: document.getElementById('maintenance-date').value,
            service: document.getElementById('service-type').value,
            cost: document.getElementById('cost').value.replace(/[^0-9]/g, ''),
            notes: document.getElementById('notes').value.trim()
        });
        saveToLocalStorage();
        displayedLogDate = new Date(document.getElementById('maintenance-date').value.replace(/-/g, '/'));
        renderAll();
        form.reset();
        flatpickr("#maintenance-date").setDate(new Date());
    });

    saveEditBtn.addEventListener('click', () => {
        if (!activeRecordId) return;
        const recordIndex = maintenanceRecords.findIndex(r => r.id === activeRecordId);
        if (recordIndex === -1) return;
        maintenanceRecords[recordIndex] = {
            ...maintenanceRecords[recordIndex],
            room: document.getElementById('edit-room-number').value,
            date: document.getElementById('edit-maintenance-date').value,
            service: document.getElementById('edit-service-type').value,
            cost: document.getElementById('edit-cost').value.replace(/[^0-9]/g, ''),
            notes: document.getElementById('edit-notes').value.trim()
        };
        saveToLocalStorage();
        displayedLogDate = new Date(document.getElementById('edit-maintenance-date').value.replace(/-/g, '/'));
        renderAll();
        closeRecordModal();
        if (!monthDetailsModal.classList.contains('hidden')) {
             const [_, month, year] = monthDetailsTitle.textContent.match(/(\d+)\/(\d+)/);
             openMonthDetailsModal(month - 1, year);
        }
    });

    deleteRecordBtn.addEventListener('click', () => {
        if (!activeRecordId) return;
        maintenanceRecords = maintenanceRecords.filter(r => r.id !== activeRecordId);
        saveToLocalStorage();
        renderAll();
        closeRecordModal();
        if (!monthDetailsModal.classList.contains('hidden')) {
             const [_, month, year] = monthDetailsTitle.textContent.match(/(\d+)\/(\d+)/);
             openMonthDetailsModal(month - 1, year);
        }
    });

     // Data Import / Export Listeners
    exportDataBtn.addEventListener('click', () => {
        if (maintenanceRecords.length === 0) {
            alert("Không có dữ liệu để xuất.");
            return;
        }
        const dataString = JSON.stringify(maintenanceRecords, null, 2);
        const blob = new Blob([dataString], {type: 'application/json'});
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        const today = new Date().toISOString().slice(0, 10);
        a.href = url;
        a.download = `baotri_backup_${today}.json`;
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
        URL.revokeObjectURL(url);
    });
    
    openExportMonthBtn.addEventListener('click', openExportMonthModal);
    cancelExportMonthBtn.addEventListener('click', closeExportMonthModal);
    confirmExportMonthBtn.addEventListener('click', () => {
        const selectedMonth = exportSelectMonth.value;
        const selectedYear = exportSelectYear.value;
        exportMonthToCsv(selectedMonth, selectedYear);
        closeExportMonthModal();
    });

    importDataBtn.addEventListener('click', () => importFileInput.click());

    importFileInput.addEventListener('change', (event) => {
        const file = event.target.files[0];
        if (!file) return;
        const reader = new FileReader();
        reader.onload = (e) => {
            try {
                const importedData = JSON.parse(e.target.result);
                if (!Array.isArray(importedData)) throw new Error("Tệp không chứa dữ liệu mảng (array).");
                if (importedData.length > 0 && (!importedData[0].id || !importedData[0].date)) {
                     throw new Error("Dữ liệu trong tệp không đúng định dạng.");
                }
                dataToImport = importedData;
                importFilenameEl.textContent = file.name;
                importConfirmModal.classList.remove('hidden');
            } catch (error) {
                alert(`Lỗi khi đọc tệp: ${error.message}`);
                dataToImport = null;
            } finally {
                 importFileInput.value = '';
            }
        };
        reader.readAsText(file);
    });

    cancelImportBtn.addEventListener('click', () => {
        importConfirmModal.classList.add('hidden');
        dataToImport = null;
    });

    confirmImportBtn.addEventListener('click', () => {
        if (dataToImport) {
            maintenanceRecords = dataToImport;
            saveToLocalStorage();
            renderAll();
            importConfirmModal.classList.add('hidden');
            dataToImport = null;
        }
    });

    // Modal listeners
    closeMonthDetailsBtn.addEventListener('click', closeMonthDetailsModal);
    cancelEditBtn.addEventListener('click', closeRecordModal);
    openReportBtn.addEventListener('click', openReportModal);
    closeReportBtn.addEventListener('click', closeReportModal);
    
    [costInput, document.getElementById('edit-cost')].forEach(input => input.addEventListener('input', () => input.value = input.value.replace(/[^0-9]/g, '')));
    
    // --- INITIALIZATION ---
    flatpickr("#maintenance-date", { locale: "vn", altInput: true, altFormat: "d/m/Y", dateFormat: "Y-m-d", defaultDate: "today" });
    renderAll();
});
</script>

</body>
</html>
