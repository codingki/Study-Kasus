Study Kasus
Aplikasi sederhana untuk kasir

Kegunaan
User dapat memilih, serta menghapus item pada keranjang
User dapat memilih voucher untuk pemotongan harga
Algoritma
User pertama kali akan ditampilkan halaman mainwindow yang berisikan keranjang, serta subtotal dan total harga. Serta opsi untuk memilih voucher yang tersedia. kemudian user akan memilih item pada halaman penawaran yang kemudian item tersebut akan masuk ke dalam keranjang (user dapat menghapus item jika ada kesalahan) Lanjut dengan pemilihan voucher sebagai akhir dari alur aplikasi serta penggunaannya untuk pemotongan harga pada total.

    public MainWindow()
    {
        InitializeComponent();

        payment = new Payment(this);

        KeranjangBelanja keranjangBelanja = new KeranjangBelanja(payment, this);

        controller = new MainWindowController(keranjangBelanja);

        listBoxPesanan.ItemsSource = controller.getSelectedItems();
        listBoxPakaiVoucher.ItemsSource = controller.getSelectedVouchers();

        initializeView();
Disini kurang lebih baris-baris yang akan mempengaruhi jalannya progam. Dimana item akan dimasukkan kedalam listbox, juga voucher tersebut. sehingga user dapat menemukan item maupun voucher tersebut

Item-item yg akan dilihat user dimasukkan kedalam listbox sendiri, juga dengan voucher. Setiap event yang di klik pada voucher berisi pemotongan harga secara algoritma. begitu juga dengan item-item makanan dimana setiap item ditambahkan harga akan semakin bertambah, juga saat menghapus item, harga akan mengurang.

private void OnPilihVoucherClicked(object sender, RoutedEventArgs e)
{
    PilihVoucher pilihVoucherWindow = new PilihVoucher();
    pilihVoucherWindow.SetOnItemSelectedListener(this);
    pilihVoucherWindow.Show();
}
Button voucher yang ditekan akan memunculkan halaman baru. Yaitu daftar voucher.

private void onButtonAddItemClicked(object sender, RoutedEventArgs e)
{
    Penawaran penawaranWindow = new Penawaran();
    penawaranWindow.SetOnItemSelectedListener(this);
    penawaranWindow.Show();
}
Button tambah yang ditekan akan memunculkan halaman baru. Yaitu daftar item penawaran

private void listBoxPesanan_ItemClicked(object sender, MouseButtonEventArgs e)
{
    if (MessageBox.Show("Kamu ingin menghapus item ini?",
            "Konfirmasi", MessageBoxButton.YesNo) == MessageBoxResult.Yes)
    {
        ListBox listBox = sender as ListBox;
        Item item = listBox.SelectedItem as Item;
        controller.deleteSelectedItem(item);
    }
}
Penghapusan item saat item pada list di tekan dan dikonfirmasi penghapusannya.

public void onPriceUpdated(double subtotal,  double grantTotal, double potongan)
{
    labelSubtotal.Content = "Rp " + subtotal;
    labelGrantTotal.Content = "Rp " + grantTotal;
    labelPromoFee.Content = "Rp " + potongan;
}
Semua proses yang terjadi pada harga akan ditampilkan.
