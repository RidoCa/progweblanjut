## Prasyarat

1. Project dari Minggu ke 6
1. ng serve
<hr/>
Setelah pada materi Local Reference kita sudah mengetahui cara membuat variabel pada template dan memanfaatkannya, kali ini kita akan menggunakan decorator @ViewChild untuk mengakses langsung element pada template dari file TS. Modifikasi kode template komponen cockpit menjadi seperti dibawah ini,

```html
<div class="row">
  <div class="col-xs-12">
    <p>Add new Servers or blueprints!</p>
    <label>Server Name</label>
    <!-- <input type="text" class="form-control" [(ngModel)]="newServerName"> -->
    <input 
      type="text" 
      class="form-control" 
      #serverNameInput>
    <label>Server Content</label>
    <!-- <input type="text" class="form-control" [(ngModel)]="newServerContent"> -->
    <input 
      type="text" 
      class="form-control"
      #serverContentInput>
    <br>
    <button
      class="btn btn-primary"
      (click)="onAddServer(serverNameInput)">Add Server</button>
    <button
      class="btn btn-primary"
      (click)="onAddBlueprint(serverNameInput)">Add Server Blueprint</button>
  </div>
</div>
```
Pada baris ke-12 sampai ke-15, kita menggantikan input **newServerContent** yang sebelumnya menggunakan two-way binding, menjadi menggunakan local reference bernama **serverContentInput**. Selanjutnya kita akan mengkases element input dengan local reference **serverContentInput** dengan menggunakan decorator @ViewChild pada file TS. Modifikasi file TS komponen cockpit menjadi,

```typescript
import { Component, OnInit, EventEmitter, Output, ViewChild, ElementRef } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent:string}>();
  @Output('bpCreated') blueprintCreated = new EventEmitter<{blueprintName: string, blueprintContent:string}>();
  // newServerName = '';
  // newServerContent = '';
  @ViewChild('serverContentInput') serverContentInput: ElementRef;

  constructor() { }

  ngOnInit() {
  }

  onAddServer(nameInput: HTMLInputElement) {
    this.serverCreated.emit({
      // serverName: this.newServerName,
      serverName: nameInput.value,
      serverContent: this.serverContentInput.nativeElement.value
    })
  }

  onAddBlueprint(nameInput: HTMLInputElement) {
    this.blueprintCreated.emit({
      // blueprintName: this.newServerName,
      blueprintName: nameInput.value,
      blueprintContent: this.serverContentInput.nativeElement.value
    })
  }

}
```
Pada baris ke-11 dan ke-12, kita komen terlebih dahulu variable **newServerName** dan **newServerContent**. Kedua variabel tersebut sudah tidak kita butuhkan saat ini karena kita tidak akan menggunakan two-way binding. Kemudian pada baris ke-13, kita mendifinisikan **serverContentInput** dengan decorator @ViewChild(). Pada @ViewChild() terdapat **argumen 'serverContentInput'**. **Argument tersebut merupakan nama dari local reference yang ada pada template**. Tipe **ElementRef** yang digunakan, merupakan penanda bahwa serverContentInput adalah sebuah element DOM / element HTML. Perhatikan juga baris ke-1, @ViewChild harus kita import terlebih dahulu dari @angular/core.
<p>Untuk memanfaatkan value dari @ViewChild, perhatikan baris ke-24 dan ke-32 pada fungsi onAddServer dan onAddBlueprint. Kita dapat dengan mudah mengakses value dari input element.</p>