Local reference dalam Angular merupakah sebuah variabel yang hanya dapat digunakan di dalam sebuah template pada segala elemen HTML. Pada topik ini kita akan mencoba mengimplementasikan local reference pada komponen cockpit. Modifikasi template cockpit menjadi,
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
    <input type="text" class="form-control" [(ngModel)]="newServerContent">
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
Perhatikan baris ke-9. Kita menggunakan **#serverNameInput** yang merupakan local reference. Local reference ditadai dengan tanda '**#**'. Karena kita sudah membuat local reference, maka dapat kita gunakan dalam template. Perhatikan baris ke-15 dan ke-18. Kita dapat menggunakan **serverNameInput** sebagai parameter dari fungsi onAddServer() dan onAddBlueprint. Selanjutnya, karena onAddServer() dan onAddBlueprint pada pembahasan sebelumnya belum kita set untuk menerima parameter, maka kita akan memodifikasi kode pada file TS cockpit. Ubah file TS cockpit menjadi,
```typescript
import { Component, OnInit, EventEmitter, Output } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent:string}>();
  @Output('bpCreated') blueprintCreated = new EventEmitter<{blueprintName: string, blueprintContent:string}>();
  newServerName = '';
  newServerContent = '';

  constructor() { }

  ngOnInit() {
  }

  onAddServer(nameInput: HTMLInputElement) {
    this.serverCreated.emit({
      // serverName: this.newServerName,
      serverName: nameInput.value,
      serverContent: this.newServerContent
    })
  }

  onAddBlueprint(nameInput: HTMLInputElement) {
    this.blueprintCreated.emit({
      // blueprintName: this.newServerName,
      blueprintName: nameInput.value,
      blueprintContent: this.newServerContent
    })
  }

}
```
Parameter **nameInput** diberikan pada fungsi onAddServer() dan onAddBlueprint() karena kita tau, bahwa pada template, kita melakukan passing paramter serverNameInput. Paramter **nameInput** bertipe HTMLElementInput karena kita tahu pada template, local reference kita sematkan dalam element input. Sehingga pada baris ke-22 dan ke-30, kita dapat mengganti variabel newServerName menjadi parameter dari fungsi masing masing. Hal ini dikarenakan kita sudah menggunakan local reference, sehingga tidak perlu lagi menggunakan two-way binding dan variabel pada file TS.
