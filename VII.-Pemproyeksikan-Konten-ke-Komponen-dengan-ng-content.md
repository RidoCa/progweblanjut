Pada materi ini, kita akan mencoba untuk memproyeksikan konten dari template ke komponen yang lain. Dalam materi ini, diasumsikan kode based Anda sudah melalui tahapan pada materi ke 6. Langsung saja, kita akan memindahkan element **p** pada template server-element ke app komponen, sehingga kode server-element.component.html dan app.component.html menjadi seperti berikut,
<br/>
**server-element.componen.html**
```html
<div class="panel panel-default">
  <div class="panel-heading">{{ element.name }}</div>
  <div class="panel-body">
    
  </div>
</div>
```
<br/>

**app.component.html**

```html
<div class="container">
  <app-cockpit 
    (serverCreated)="onServerAdded($event)"
    (bpCreated)="onBlueprintAdded($event)"></app-cockpit>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element 
        *ngFor="let serverElement of serverElements"
        [srvElement]=serverElement>
        <p>
          <strong *ngIf="serverElement.type === 'server'" style="color: red">{{ serverElement.content }}</strong>
          <em *ngIf="serverElement.type === 'blueprint'">{{ serverElement.content }}</em>
        </p>
      </app-server-element>
    </div>
  </div>
</div>
```
Perhatikan kode elemen **p** pada template app! Kita harus mengubah obyek variable **element** menjadi **serverElement** karena element tidak dikenali oleh komponen app. Sedangkan serverElement adalah obyek yang mirip dengan element dan dikenali oleh komponen app.
<br/>
Jalankan server Angular dengan ```ng serve``! Tambahkan server atau blueprint baru, apa yang akan terjadi ?
> Konten tidak keluar ! Hal ini karena secara default komponen hanya akan menampilkan konten pada templatenya sendiri. Sedangkan pada template app, komponen <app-server-element> didalamnya terdapat tag elemen P yang merupakan sebuah konten. Sehingga elemen P tidak dapat ditampilkan.

Untuk mengatasi hal tersebut kita harus memproyeksikan konten p ke template server-element. Caranya adalah dengan menggunakan directive ng-content. Ubah kode template server-component menjadi seperti berikut,
```html
<div class="panel panel-default">
  <div class="panel-heading">{{ element.name }}</div>
  <div class="panel-body">
    <!--
      Element <p> pada template server element dipindah dalam template app
      Secara default, Angular tidak akan menampilkan konten yang ada didalamnya,
      karena memang tidak dapat diakses.
      Untuk melakukan hal tersebut, kita memerlukan proyeksi konten dari app ke serve-element,
      dengan menggunakan directive ng-content
    -->
    <ng-content></ng-content>
  </div>
</div>
```