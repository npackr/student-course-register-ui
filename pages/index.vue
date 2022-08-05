<template>
  <v-row justify="center" align="top">
    <v-row justify="center">
      <v-col cols="12" sm="12" md="6" lg="3" xl="3">
        <v-autocomplete label="Chọn khóa"></v-autocomplete>
        <v-card class="regis-info">
          <ul>

            <li>Số TC tối thiểu cần ĐK: 0</li>
            <li>Số TC tối đa được phép ĐK: Không hạn chế</li>
            <li>Tổng TC tối đa trong kỳ: 31</li>
            <li>Số TC được phép ĐK khi bị CBHV/HL yếu: 14 </li>
          </ul>
        </v-card>
      </v-col>
      <v-col cols="12" sm="12" md="6" lg="3" xl="3">
        <v-autocomplete label="Chọn ngành"></v-autocomplete>
        <v-card class="regis-info">
          <ul>
            <li>Số TC cần ĐK khi bị CBHV: Không hạn chế</li>
            <li>Hạn chế số SV tối đa : Có</li>
            <li>Cho phép đ.ký ngoài ngành: Không</li>
            <br />
          </ul>
        </v-card>
      </v-col>
      <v-col cols="12" sm="12" md="6" lg="3" xl="3">
        <v-autocomplete label="Chọn học phần"></v-autocomplete>
        <v-card class="regis-info">
          <ul>
            <li>Hạn đăng ký: 9h, 06/07/2022 -> 10/07/2022</li>
            <li>Hạn hủy: 11/07/2022 -> 13/07/2022 chỉ hủy không đăng ký</li>
            <br />
          </ul>
        </v-card>
      </v-col>
      <v-col cols="12" sm="12" md="6" lg="3" xl="3">
        <v-menu ref="menu" v-model="menu" :close-on-content-click="false" :return-value.sync="date"
          transition="scale-transition" offset-y min-width="auto">
          <template v-slot:activator="{ on, attrs }">
            <v-text-field v-model="date" label="Chọn ngày học" prepend-icon="mdi-calendar" readonly v-bind="attrs"
              v-on="on"></v-text-field>
          </template>
          <v-date-picker v-model="date" no-title scrollable>
            <v-spacer></v-spacer>
            <v-btn text color="primary" @click="menu = false">
              Cancel
            </v-btn>
            <v-btn text color="primary" @click="$refs.menu.save(date)">
              OK
            </v-btn>
          </v-date-picker>
        </v-menu>
        <v-btn block color="primary" class="regis-btn">
          Lọc lớp không trùng th.g
        </v-btn>
        <v-btn outlined block color="primary" class="regis-btn">
          In kết quả
        </v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" sm="12" md="12" lg="12" xl="6">
        <div class="justify-center">
          <h2>Danh sách lớp học phần có thể đăng ký học kỳ 2 năm học 2021_2022 đợt học 6</h2>
          <p>Những lớp học phần thuộc cùng một dải màu liên tiếp dạy cùng một học phần, chỉ được chọn không quá 1 lớp
          </p>
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" sm="12" md="12" lg="12" xl="6">
        <v-card class="logo py-6 d-flex justify-center">
          Danh sách lớp đang mở
        </v-card>
        <div>
          <v-data-table :headers="headers" :items="desserts" class="elevation-1">
            <template v-slot:[`item.glutenfree`]="{ item }">
              <v-simple-checkbox v-model="item.glutenfree" disabled></v-simple-checkbox>
            </template>
          </v-data-table>
        </div>
      </v-col>
      <v-col cols="12" sm="12" md="12" lg="12" xl="12">
        <v-card class="logo py-6 d-flex justify-center">
          Danh sách lớp đã đăng ký
        </v-card>
        <div>
          <v-data-table :headers="dessertHeaders" :items="desserts" :single-expand="singleExpand"
            :expanded.sync="expanded" item-key="name" show-expand class="elevation-1">
            <template v-slot:top>
              <v-toolbar flat>
                <v-toolbar-title></v-toolbar-title>
                <v-spacer></v-spacer>
                <v-switch v-model="singleExpand" label="Chỉ xem một mục mỗi lần" class="mt-2"></v-switch>
              </v-toolbar>
            </template>
            <template v-slot:expanded-item="{ headers, item }">
              <td style="padding: 24px;" :colspan="headers.length">
                <h3>Thông tin chi tiết môn {{ item.carbs }}</h3>
                {{ item.chitiet }}
                <div style="float: right; margin: 24px;">
                  <v-btn color="danger" elevation="2">Hủy đăng ký</v-btn>
                </div>
              </td>
            </template>
          </v-data-table>
        </div>
      </v-col>
    </v-row>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  date: new Date(),
  data() {
    return {
      expanded: [],
      singleExpand: false,
      dessertHeaders: [
        {
          text: 'Mã lớp',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Học phần', value: 'carbs' },
        { text: 'Thời gian', value: 'protein' },
        { text: 'Địa điểm', value: 'iron' },
        { text: 'Tín chỉ', value: 'tinchi' },
        { text: 'Học phí', value: 'hocphi' },
        { text: '', value: 'data-table-expand' },
      ],
      desserts: [
        {
          name: '1',
          calories: 159,
          fat: "6/48",
          carbs: "Tương tác người máy",
          protein: "Từ 1/8 đến 21/9: Thứ 2, 4, 6 - Tiết 1, 2, 3",
          iron: 'Phòng 602-SG',
          glutenfree: true,
          tinchi: 3,
          hocphi: '5.000.000',
          chitiet: "Tương tác người–máy bao gồm công việc nghiên cứu, hoạch định, thiết kế và khai thác sự giao tiếp giữa con người và máy tính. Đây thường được coi là ngành giao thoa giữa khoa học máy tính, khoa học hành vi, thiết kế, nghiên cứu truyền thông và một số lĩnh vực nghiên cứu khác. "
        },
        {
          name: '2',
          calories: 237,
          fat: "7/48",
          carbs: "Giải tích hàm một biến",
          protein: "Từ 1/8 đến 21/9: Thứ 2, 4, 6 - Tiết 1, 2, 3",
          iron: 'Phòng 603-SG',
          glutenfree: false,
          tinchi: 3,
          hocphi: '5.000.000',
          chitiet: ""
        },
        {
          name: '3',
          calories: 262,
          fat: "5/48",
          carbs: "Lịch sử Đảng cộng sản Việt Nam",
          protein: "Từ 1/8 đến 21/9: Thứ 2, 4, 6 - Tiết 1, 2, 3",
          iron: 'Phòng 703-SG',
          glutenfree: false,
          tinchi: 3,
          hocphi: '5.000.000',
          chitiet: ""
        },
      ],
      headers: [
        {
          text: 'STT',
          align: 'start',
          sortable: false,
          value: 'name',
        },
        { text: 'Mã lớp', value: 'calories' },
        { text: 'Sĩ số', value: 'fat' },
        { text: 'Học phần', value: 'carbs' },
        { text: 'Thời gian', value: 'protein' },
        { text: 'Địa điểm', value: 'iron' },
        { text: 'Đăng ký', value: 'glutenfree' },
      ],
    }
  },
}
</script>
<style>
.regis-info {
  padding: 18px;
}

.regis-btn {
  margin-bottom: 12px;
}
</style>