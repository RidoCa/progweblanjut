## Bahan Praktikum
1. Download starter-kit project di url
2. ng serve
## Memisahkan Komponen
Sebelum kita masuk pada topik Data Binding Advance, sebelumnya kita akan melakukan pemisahan komponen dari start-project. Jalankan terlebih dahulu starter-project dengan perintah ```ng serve```. Perhatikan hasil project! Terdapat bagian untuk **Add Server** dan **Add Blueprint** dengan hasil yang berbeda. Pertama-tama kita akan memisahkan kedua bagian tersebut menjadi 2 komponen yang berbeda. Perhatikan kode template komponen **app**

```html
<div class="container">
  <div class="row">
    <div class="col-xs-12">
      <p>Add new Servers or blueprints!</p>
      <label>Server Name</label>
      <input type="text" class="form-control" [(ngModel)]="newServerName">
      <label>Server Content</label>
      <input type="text" class="form-control" [(ngModel)]="newServerContent">
      <br>
      <button
        class="btn btn-primary"
        (click)="onAddServer()">Add Server</button>
      <button
        class="btn btn-primary"
        (click)="onAddBlueprint()">Add Server Blueprint</button>
    </div>
  </div>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <div
        class="panel panel-default"
        *ngFor="let element of serverElements">
        <div class="panel-heading">{{ element.name }}</div>
        <div class="panel-body">
          <p>
            <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
            <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
          </p>
        </div>
      </div>
    </div>
  </div>
</div>
```
Kemudian, buatlah komponen baru dengan menggunakan Angular CLI. Komponen pertama bernama **cockpit**, komponen ke dua bernama **server-element**.
```cli
$ ng g c cockpit --spec false
$ ng g c server-element --spec false
```
Cut baris ke-2 hingga ke-17 dari kode template app ke kode template cockpit, sehingga menjadi seperti dibawah ini.
```html
<div class="row">
    <div class="col-xs-12">
      <p>Add new Servers or blueprints!</p>
      <label>Server Name</label>
      <input type="text" class="form-control" [(ngModel)]="newServerName">
      <label>Server Content</label>
      <input type="text" class="form-control" [(ngModel)]="newServerContent">
      <br>
      <button
        class="btn btn-primary"
        (click)="onAddServer()">Add Server</button>
      <button
        class="btn btn-primary"
        (click)="onAddBlueprint()">Add Server Blueprint</button>
    </div>
  </div>
```
Akan ada underliner berwarna merah yang menandakan ada error yang terjadi.  Untuk mengatasi masalah tersebut, cut fungsi **onAddServer()** dan **onAddBluePrint()** pada kode TS komponen app ke **kode TS komponen cockpit**. Sehingga kode TS cockpit menjadi seperti berikut,
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

  onAddServer() {
    this.serverElements.push({
      type: 'server',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

  onAddBlueprint() {
    this.serverElements.push({
      type: 'blueprint',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

}
```
Terdapat error pada variabel serverElements, newServerName, dan newServerContent. Mengapa terjadi error ? Karena kita belum melakukan deklarasi terhadap variabel tersebut. Perhatikan kode TS komponen app **setelah** kita pindahkan fungsi onAddServer() dan onAddBlueprint() kedalam kode TS komponen cockpit.
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  serverElements = [];
  newServerName = '';
  newServerContent = '';

  
}
```
Variabel serverElements, newServerName, dan newServerContent masih tertinggal didalam kode TS app. Pada materi ini kita hanya akan memindahkan newServerName dan newServerContent kedalam kode TS cockipt. Array serverElements[] nantinya akan kita akses dari komponen lain, dimana secara default tidak dilakukan oleh Angular. Sampai tahap ini, kode TS dari cockpit dan app adalah sebagai berikut, <br/><br/>

**cockpit.component.ts**
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {
  newServerName = '';
  newServerContent = '';

  constructor() { }

  ngOnInit() {
  }

  onAddServer() {
    this.serverElements.push({
      type: 'server',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

  onAddBlueprint() {
    this.serverElements.push({
      type: 'blueprint',
      name: this.newServerName,
      content: this.newServerContent
    });
  }

}
```
<br/><br/>
**app.component.ts**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  serverElements = [];

  
}
```
Selanjutnya, kembali ke file template komponen app, pindahkan atau cut baris ke-6 sampai ke-16 kedalam file template server-element. Sehingga file template server-element dan app menjadi seperti berikut,<br/><br/>

**server-element.component.html**
```html
<div class="panel panel-default" *ngFor="let element of serverElements">
  <div class="panel-heading">{{ element.name }}</div>
  <div class="panel-body">
    <p>
      <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
      <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
    </p>
  </div>
</div>
```
<br/><br/>
**app.component.html**
```html
<div class="container">
  
  <hr>
  <div class="row">
    <div class="col-xs-12">
      
    </div>
  </div>
</div>
```
Selanjutnya, karena kita telah memiliki komponen server-elemen secara individu dan terpisan dari komponen app, maka kita tidak memerlukan directive ngFor pada template server-element. Pada tahap selanjutnya, komponen server-element lah yang akan kita looping menggunakan ngFor, bukan variabel array serverElements[]. Kode template dari komponen server-element akan berubah menjadi seperti berikut,
```html
<div class="panel panel-default">
  <div class="panel-heading">{{ element.name }}</div>
  <div class="panel-body">
    <p>
      <strong *ngIf="element.type === 'server'" style="color: red">{{ element.content }}</strong>
      <em *ngIf="element.type === 'blueprint'">{{ element.content }}</em>
    </p>
  </div>
</div>
```
Terdapat error pada variabel element, akan kita selesaikan pada pembahasan selanjutnya. Langkah terakhir pada topik memisahkan komponen ini adalah, rubah kode template komponen app, dengan memanggil komponen server-element didalam div dengan class "col-xs-12". Perhatikan kode template app berikut,
```html
<div class="container">
  
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element *ngFor="let serverElement of serverElements"></app-server-element>
    </div>
  </div>
</div>
```