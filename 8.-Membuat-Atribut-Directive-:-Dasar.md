## Pre-requisites
1. Copy project directive-start.zip
1. Ekstrak project directive-start
1. `ng serve`

***
## Membuat Atribut Directive
Pada bagian ini kita akan mencoba untuk membuat sendiri atribut directive. Pertama-tama ikut langkah berikut ini.
1. Buatlah folder bernama **basic-highlight** di dalam folder **app**
1. Di dalam folder **basic-highlight** buatlah sebuah file bernama **basic-highlight.directive.ts**
<br/>
Selanjutnya, pada file **basic-highlight.directive.ts** ketikkan kode dibawah ini,

```typescript
import { Directive, ElementRef, OnInit } from "@angular/core";

@Directive({
    selector:'[appBasicHighlight]'
})
export class BasicHighlightDirective implements OnInit {
    constructor(private elementRef: ElementRef){
    }

    ngOnInit(){
        this.elementRef.nativeElement.style.backgroundColor = 'green';
    }
}
```
Perhatikan baris ke 3. Untuk memastikan bahwa atribut directive yang kita buat dikenali oleh angular, maka kita harus memberikan decorator @Directive. Decorator @Directive minimal harus memiliki parameter **selector**, sehingga selector harus ditambahkan sebagai parameter pada baris ke-4. Perlu diingiat, decorator @Directive harus kita import dari @angular/core, perhatikan baris ke-1.
<br/>
Pada baris ke-6 kita membuat sebuah kelas bernama **BasicHighlightDirective**. Kelas ini kita buat agar otomatis berjalan ketika Angular sampai pada lifecycle **OnInit**, sehingga kita perlu mengimplementasikan OnInit pada kelas BasicHighlightDirective. Keyword **implements OnInit** pada kelas, mewajidbkan kita untuk mendefinisikan fungsinya, yaitu pada baris ke-10 sampai ke-12. Pada fungsi ngOnInit() kita akan membuat element yang menggunakan directive ini, akan memiliki background warna hijau.

## Mengimplementasikan Atribut Directive
Jika kalian perhatikan pada IDE yang kalian gunakan, maka pada decorator @Directive saat ini masih memiliki tanda underline merah. Hal ini menandakan bahwa terdapat error pada decorator tersebut. Error yang dimaksud adalah, _**kita belum melakukan import BasicHighlightDirective di app.module.ts**_ sehingga belum dikenali oleh Angular. Masih ingat cara import FormsModules atau komponen secara manual ? Lakukan import kelas BasicHighlightDirective pada app.module.ts sehingga kode app.module.ts menjadi,
```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';
import { BasicHighlightDirective } from './basic-highlight/basic-highlight.directive';

@NgModule({
  declarations: [
    AppComponent,
    BasicHighlightDirective
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
Berbeda dengan komponen yang harus kita masukkan dalam array imports, untuk directive harus kita masukkan dalam array **declaration**. Perhatikan baris ke-12. Sampai tahapan ini, kita sudah selesai membuat atribut directive. Selanjutnya kita akan mengimplementasikannya pada file template. Ubah kode template app menjadi,
```html
<div class="container">
  <div class="row">
    <div class="col-xs-12">
      <button
        class="btn btn-primary"
        (click)="onlyOdd = !onlyOdd">Only show odd numbers</button>
      <br><br>
      <ul class="list-group">
        <div *ngIf="onlyOdd">
          <li
            class="list-group-item"
            [ngClass]="{'odd': odd % 2 !== 0}"
            [ngStyle]="{'background-color': odd % 2 !== 0 ? 'yellow' : 'transparent'}"
            *ngFor="let odd of oddNumbers">
            {{ odd }}
          </li>
        </div>
        <div *ngIf="!onlyOdd">
          <li
            class="list-group-item"
            [ngClass]="{'odd': even % 2 !== 0}"
            [ngStyle]="{'background-color': even % 2 !== 0 ? 'yellow' : 'transparent'}"
            *ngFor="let even of evenNumbers">
            {{ even }}
          </li>
        </div>
      </ul>
      <p appBasicHighlight>Style me with basic directive</p>
    </div>
  </div>
</div>
```
Perhatikan baris ke-28. Kita menambahkan sebuah paragraf dengan menggunakan atribut directive yang kita buat sendiri.