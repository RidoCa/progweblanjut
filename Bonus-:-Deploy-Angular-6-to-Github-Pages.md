Berikut merupakan tutorial deploy Angular ke Github Pages

### Membuat Repository Github Pages
Buat repository baru dengan nama [username_github].github.io. Contoh, username github adalah **progwebpolinema** maka repository github pages bernama **progwebpolinema.github.io**. Biarkan repository kosong!

### Upload Project Angular ke Github Pages
#### Step 1
Buat repository baru untuk project Angular
#### Step 2
1. Build project Angular dengan perintah ```ng build `nama project` --vendor-chunk=true```
1. Push project Angular pada komputer lokal ke remote repository github yang telah dibuat pada **step 1**
### Deploy Angular ke Github Pages
1. Install package angular github pages dengan perintah ```npm i -g angular-cli-ghpages```
2. Build ke remote repo dengan perintah ```ng build --prod --base-href https://[username_github].github.io/[nama_repository]/```. Sesuaikan username github dan nama repository.
3. (Optional) Jika gagal, build kembali dengan perintah ng build --prod --base-href=/[nama_repository]/
4. Deploy dengan perintah ```ngh --dir=dist/[nama_project]```

### Hasil
Contoh hasil deploy Angular ke Github Pages dapat dilihat di [https://progwebpolinema.github.io/kuis/](https://progwebpolinema.github.io/kuis/)