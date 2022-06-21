** pour faire le lien : 
dans le routing 
const routes: Routes = [
  {path:'addDoc', component:AddDoctorComponent},
  {path:'Home', component:HomeComponent},
];
apres dans app.html :
<a [routerLink]="['Home']">Acceuil</a>
<router-outlet></router-outlet>

**add component : ng g c nomcomp
**add class : ng g class nomclass
**add service: ng g s nomservice
**add service dans un dossier: ng g s nomdosier/nomservice
**showdoc: dans .ts show -> @Input()d!:Doctor (avant constructor)
=> apres dans show.html : {{ d.id}} de tt les attributs
=> demandé laffi dans home donc :
dans home.html : <app-show-one-doctor></app-show-one-doctor>
=>dans app.ts : imports : add : httpClientModule
=>dans app.ts: import { HttpClientModule } from '@angular/common/http';
=> dossier service ->servie.ts:constructor(private http:HttpClient){}
=> then add method :getDoctor(){
      return this.http.get('http://localhost:3000/doctor');
    }
=> dans bachend/db.json : supp all just:
{
  "doctor": [
    ]
  
}
**methode add doctor:dans doctor.service.ts
addDoctor(data:any){
    return this.http.post('http://localhost:3000/doctor', data);
=>puis dans le composant adddoc.html :
-> ajout du formulaire (à voir) et dans app.ts dans imports: formsModule 
**controle saisie formulaire:
champ obligatoire on met: required 
exmpl: 
<input type="number" name="id" ngModel required>
=> champ contient min 10 carac:
<input type="text" name="domaine" ngModel minlength="10">
=> afficher un msg adequat:
<input type="number" name="id" ngModel required #n="ngModel">
(n pr dire name..) var de ref, dans les champ avc control,
=>test pr aff le msg
<input type="number" name="id" ngModel required #n="ngModel">
<div *ngIf="n.invalid">
  required (ay msg)
</div>
**add doc:
en cliquant sur save, un redirection: donc on implement la methode
de clic:
<input type="submit" value="save" (click)="savedata(f.value)">
puis dans add doc comp:
une meth apres ngoninit:
savedata(f:any){}
et dans le constructor: constructor(private doc:DoctorService, private router:Router) { }
puis dans la meth save..
  savedata(f:any){
    this.doc.addDoctor(f).subscribe(
      ()=>{
    this.router.navigate(['home']);
      }
    );
  }
**pr aff la list dans home:
=>dans home.ts: constructor(private s: DoctorService) { }
=>avant constructor: doct!:Doctor[]; (doc! ay var)
=>puis dans la meth ngoninit:
ngOnInit(): void {
    this.s.getDoctor().subscribe(
      (d)=>{
        this.doct=d;
      }
    );
  }
=> puis dans home.html:(pr afficher la list)
<p>home works!</p>
<div *ngFor="let test of doct">
<app-show-one-doctor [d]="test"></app-show-one-doctor>
</div>
(pr recuperer les donnees)


json-server --watch db.json
del.ts:
import { Component, OnInit } from '@angular/core';
import {DoctorService} from "../service/doctor.service";

@Component({
  selector: 'app-deldoctor',
  templateUrl: './deldoctor.component.html',
  styleUrls: ['./deldoctor.component.css']
})
export class DeldoctorComponent implements OnInit {

  constructor(private s:DoctorService) { }

  ngOnInit(): void {
  }

}
============
doc service.ts:
 deldoctor(id:any) {
    return this.http.delete('http://localhost:3000/doctor/'+id)
  }
==========
  home.ts:
  after(j:any){
this.s.deldoctor(j).subscribe(
  ()=>{
    alert('deleted')
  }
);
  }
============
routing:
{path:'del/:id',component:DeldoctorComponent},
============
deldoccomp.ts
import { Component, OnInit } from '@angular/core';
import {DoctorService} from "../service/doctor.service";

@Component({
  selector: 'app-deldoctor',
  templateUrl: './deldoctor.component.html',
  styleUrls: ['./deldoctor.component.css']
})
export class DeldoctorComponent implements OnInit {

  constructor(private s:DoctorService) { }

  ngOnInit(): void {
  }

}# pidev
