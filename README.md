#Game Othello

class Othello:
    def __init__(self):
        self.papan_permainan = [[' ' for _ in range(8)] for _ in range(8)]
        self.papan_permainan[3][3] = self.papan_permainan[4][4] = 'P'
        self.papan_permainan[3][4] = self.papan_permainan[4][3] = 'H'
        self.pemain_saat_ini = 'H'

    def print_papan_permainan(self):
        print(" " + " ".join(map(str, range(8))))
        for i, baris in enumerate(self.papan_permainan):
            print(i," ".join(baris))
            print()

    def print_skor(self):
        skor_hitam = sum(baris.count('H') for baris in self.papan_permainan)
        skor_putih = sum(baris.count('P') for baris in self.papan_permainan)
        print(f"Skor: Hitam (H) = {skor_hitam}, Putih (P) = {skor_putih}")

    def apakah_gerakan_valid(self, baris, kolom, pemain):
        if self.papan_permainan[baris][kolom] != ' ':
            return False
        lawan = 'H' if pemain == 'P' else 'P'
        arah = [(-1, 0), (1, 0), (0, -1), (0, 1), (-1, -1), (1, 1), (-1, 1), (1, -1)]
        for db, dk in arah:
            b, k = baris + db, kolom + dk
            lawan_ditemukan = False
            while 0 <= b < 8 and 0 <= k < 8 and self.papan_permainan[b][k] == lawan:
                lawan_ditemukan = True
                b += db
                k += dk
            if lawan_ditemukan and 0 <= b < 8 and 0 <= k < 8 and self.papan_permainan[b][k] == pemain:
                return True
        return False

    def buat_gerakan(self, baris, kolom):
        if not self.apakah_gerakan_valid(baris, kolom, self.pemain_saat_ini):
            return False
        self.papan_permainan[baris][kolom] = self.pemain_saat_ini
        lawan = 'H' if self.pemain_saat_ini == 'P' else 'P'
        arah = [(-1, 0), (1, 0), (0, -1), (0, 1), (-1, -1), (1, 1), (-1, 1), (1, -1)]
        for db, dk in arah:
            b, k = baris + db, kolom + dk
            cakram_dibalik = []
            while 0 <= b < 8 and 0 <= k < 8 and self.papan_permainan[b][k] == lawan:
                cakram_dibalik.append((b, k))
                b += db
                k += dk
            if 0 <= b < 8 and 0 <= k < 8 and self.papan_permainan[b][k] == self.pemain_saat_ini:
                for bb, kk in cakram_dibalik:
                    self.papan_permainan[bb][kk] = self.pemain_saat_ini
        self.pemain_saat_ini = 'H' if self.pemain_saat_ini == 'P' else 'P'
        return True

    def gerakan_valid(self, pemain):
        for baris in range(8):
            for kolom in range(8):
                if self.apakah_gerakan_valid(baris, kolom, pemain):
                    return True
        return False

    def pemenang(self):
        skor_hitam = sum(baris.count('H') for baris in self.papan_permainan)
        skor_putih = sum(baris.count('P') for baris in self.papan_permainan)
        if skor_hitam > skor_putih:
            return 'H'
        elif skor_putih > skor_hitam:
            return 'P'
        return 'seri'

    def play(self):
        print("Welcome to Othello!")
        while self.gerakan_valid('H') or self.gerakan_valid('P'):
            self.print_papan_permainan()
            self.print_skor()
            print(f"giliran pemain {self.pemain_saat_ini}")
            if not self.gerakan_valid(self.pemain_saat_ini):
                print(f"Tidak ada gerakan yang valid untuk {self.pemain_saat_ini}. langkah akan dilewati.")
                self.pemain_saat_ini = 'H' if self.pemain_saat_ini == 'P' else 'P'
                continue
            try:
                baris, kolom = map(int, input("Masukkan baris dan kolom (misalnya, 3 4): ").split())
                if not self.buat_gerakan(baris, kolom):
                    print("Langkah tidak valid. Coba lagi.")
            except (ValueError, IndexError):
                print("Input tidak valid. Masukkan dua angka antara 0 dan 7 yang dipisahkan oleh spasi.")
        self.papan_permainan()
        self.print_skor()
        pemenang= self.dapatkan_pemenang()
        print(f"Game Selesai! Pemenang: {pemenang}")

if __name__ == "__main__":
    game = Othello()
    game.play()
