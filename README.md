# Bai-TH1
#include <iostream>
#include <string>
#include <iomanip>
#include <algorithm>
#include <cctype>

using namespace std;

class Xe {
private:
    int maXe;
    string loaiXe;
    int gioVao;
    int thoiGianDo;
    double phiDoXe;
    bool dangChiemCho;

public:
    Xe();
    Xe(int maXe, const string& loaiXe, int gioVao, int thoiGianDo);

    int layMaXe() const;
    string layLoaiXe() const;
    int layGioVao() const;
    int layThoiGianDo() const;
    double layPhiDoXe() const;
    bool layDangChiemCho() const;

    void ganMaXe(int maXe);
    void ganLoaiXe(const string& loaiXe);
    void ganGioVao(int gioVao);
    void ganThoiGianDo(int thoiGianDo);
    void ganPhiDoXe(double phiDoXe);
    void ganDangChiemCho(bool dangChiemCho);

    static string chuanHoaLoaiXe(const string& loaiXe);
    static bool laLoaiXeHopLe(const string& loaiXe);

    void tinhPhiDoXe();
    void hienThi() const;
};

Xe::Xe() {
    this->maXe = 0;
    this->loaiXe = "Car";
    this->gioVao = 0;
    this->thoiGianDo = 1;
    this->phiDoXe = 0.0;
    this->dangChiemCho = false;
}

Xe::Xe(int maXe, const string& loaiXe, int gioVao, int thoiGianDo) {
    this->maXe = maXe;
    this->loaiXe = laLoaiXeHopLe(loaiXe) ? chuanHoaLoaiXe(loaiXe) : "Car";
    this->gioVao = (gioVao >= 0 && gioVao <= 23) ? gioVao : 0;
    this->thoiGianDo = (thoiGianDo >= 1 && thoiGianDo <= 23) ? thoiGianDo : 1;
    this->dangChiemCho = true;
    this->tinhPhiDoXe();
}

int Xe::layMaXe() const {
    return this->maXe;
}

string Xe::layLoaiXe() const {
    return this->loaiXe;
}

int Xe::layGioVao() const {
    return this->gioVao;
}

int Xe::layThoiGianDo() const {
    return this->thoiGianDo;
}

double Xe::layPhiDoXe() const {
    return this->phiDoXe;
}

bool Xe::layDangChiemCho() const {
    return this->dangChiemCho;
}

void Xe::ganMaXe(int maXe) {
    this->maXe = maXe;
}

void Xe::ganLoaiXe(const string& loaiXe) { 
    if (laLoaiXeHopLe(loaiXe)) {
        this->loaiXe = chuanHoaLoaiXe(loaiXe);
        this->tinhPhiDoXe();
    }
}

void Xe::ganGioVao(int gioVao) { 
    if (gioVao >= 0 && gioVao <= 23) {
        this->gioVao = gioVao;
        this->tinhPhiDoXe();
    }
}

void Xe::ganThoiGianDo(int thoiGianDo) { 
    if (thoiGianDo >= 1 && thoiGianDo <= 23) {
        this->thoiGianDo = thoiGianDo;
        this->tinhPhiDoXe();
    }
}

void Xe::ganPhiDoXe(double phiDoXe) { 
    this->phiDoXe = phiDoXe; 
}

void Xe::ganDangChiemCho(bool dangChiemCho) { 
    this->dangChiemCho = dangChiemCho; 
}

string Xe::chuanHoaLoaiXe(const string& loaiXe) {
    if (loaiXe.empty()) return "";
    string res = loaiXe;
    transform(res.begin(), res.end(), res.begin(), ::tolower);
    res[0] = toupper(res[0]);
    return res;
}

bool Xe::laLoaiXeHopLe(const string& loaiXe) {
    string temp = chuanHoaLoaiXe(loaiXe);
    return (temp == "Car" || temp == "Motorcycle" || temp == "Bicycle");
}

void Xe::tinhPhiDoXe() {
    if (!this->dangChiemCho) {
        this->phiDoXe = 0.0;
        return;
    }

    double giaCoBanMoiGio = 0.0;
    if (this->loaiXe == "Car") giaCoBanMoiGio = 20000;
    else if (this->loaiXe == "Motorcycle") giaCoBanMoiGio = 5000;
    else if (this->loaiXe == "Bicycle") giaCoBanMoiGio = 2000;

    double tongPhi = 0.0;
    for (int i = 0; i < this->thoiGianDo; ++i) {
        int gioHienTai = (this->gioVao + i) % 24;
        if (gioHienTai >= 7 && gioHienTai < 18) {
            tongPhi += giaCoBanMoiGio * 1.5;
        } else {
            tongPhi += giaCoBanMoiGio;
        }
    }
    this->phiDoXe = tongPhi;
}

void Xe::hienThi() const {
    if (!this->dangChiemCho) {
        cout << "[Vi tri trong]\n";
        return;
    }
    cout << "-----------------------------------\n";
    cout << "Ma xe: " << this->maXe << endl;
    cout << "Loai xe: " << this->loaiXe << endl;
    cout << "Gio vao: " << this->gioVao << "h" << endl;
    cout << "Thoi gian do: " << this->thoiGianDo << " gio" << endl;
    cout << "Phi do xe: " << fixed << setprecision(0) << this->phiDoXe << " VND" << endl;
    cout << "-----------------------------------\n";
}

int main() {
    Xe xe1(101, "car", 8, 3);
    cout << "Thong tin Xe 1:" << endl;
    xe1.hienThi();

    Xe xe2;
    xe2.ganMaXe(102);
    xe2.ganLoaiXe("mOtOrCyClE");
    xe2.ganGioVao(19);
    xe2.ganThoiGianDo(2);
    xe2.ganDangChiemCho(true);

    cout << "\nThong tin Xe 2:" << endl;
    xe2.hienThi();

    return 0;
}
