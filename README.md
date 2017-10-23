# Developing Angluar 2 Front End Web Application with asp.net Web API 

Introduction
I have developed an Asp.Net MVC 5 web portal to manage the online timesheet and wage payment for my family business. Now, 
It is my time to develop an angular 2 dashboard to manage the timesheet , cash flow and wage payment for this timesheet system. 
I used angular 2 Node js NPM and visual studio code IIS web server and asp.net Web API2 in visual studio 2017 to develop 
the report management system based on the portal backend.

Get Started

A, Asp.net Web API
1, develop a new asp.net core Web API project in VS 2017
2, create some CRUD APIs to be used.
3, install IIS asp.net core hosting in window component as below
https://go.microsoft.com/fwlink/?linkid=844461
4, restart IIS web serve and publish this asp.net core web APIs to a folder such as publishoutput
5, create a virtual directory in IIS web server
6, convert this virtual directory to a web application
7, open the api link to get json data such as 
http://localhost/TimesheetAPIs/api/home/menus, this will return 
["Home","Products","Services","Components","Contacts"]

B, Angular 2 Front end
1, create an angular2 folder and install angualr 2 project template with npm command as below
⦁	npm install -g @angular/cli
⦁	ng new TimesheetReport
⦁	ng serve
2, open visual studio code to open the TimesheetReport angular 2 project for coding
3, open command prompt to run ng serve command that can watch the change I made in visual studio code 
4, test the angular 2 page in web browser with URL from ng serve such as http://localhost:4200 

Angular 2 Code in place

Angular is only doing one thing-Create HTML element and generate a DOM tree as HTML does in angular way
HTML page is composed of basic html, head, and body tags . Inside body tag we can add any elements to generate a page. 
Angular is doing in its own way to custom tags inside body tags such as <app-root></app-root>

Our Angular front deveopment is starting from here

1, create a root app component to insert a top element app-root next to body tag.
2, after root element is installed inside body tag, we need to install other html elements inside this root angular component
3, then we can add event and style to different elements 
4, HTML elements want to present business data in web page,angular invents service and http servce to get data from local and remotely data resources such as files and database. 
5, http service is the only way angular communicate with database on web.
6, typescript in angular then can consume this service via component object
7, typescript uses constructor to inject the dependency into javascript
8, this then can be used to push data from dependency into    component to present data in page.
9, element is from component, element is composed with a tree structure, top branch can see below branch, lower branch can emit data to higher branch
10,......

Root component

Now we understand that root component is a critical component we can get started angualr from here, developing root component needs to build all necessary enironment to generate this root component inside html body tag. so what enviorment is ?
1, needs a html page with this root element such as index.html
<body>
  <app-root></app-root>
</body>

2,When browser calls index.html page, it needs to parse app-root selector.

3, call app-root component, html first needs to find out it from app.module.ts file, where we import all components here includes the top level component such as 
import { AppComponent } from './app.component';

4, Browser then goes to app.component.ts file to find out the AppCOmonent class
export class AppComponent{
  title = 'Tea Journal Time sheet';
}

5, this class has a component in attribute, so we can say appcomponent is a component top-component

6, component includes
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
it means in index.html page, browser parse app-root selector and comes here to open tempate url and add style in

7, so this templateurl is a div element only, that will be embedded into index.html page
such as 
<div class="col-sm-12">
  <h1>
    {{title}}
  </h1>
  <nav>
    <a routerLink="/slot" routerLinkActive="active">Slot</a>
   </nav>
<div class=col-sm-4>
  <router-outlet></router-outlet>
</div>
   
 </div>
8, app-root selector will insert this templateurl into index.html page,  it find title data from appcomponent class, it embed router-outlet inside to host routerlink content

9, routerlink atribute should be read from app.module.ts, top level module, to see which compnent is linked and clicked, then this componet content can be inserted into router-outlet

10, /slot link will call slotproduct .ts component , so it goes to app.module.ts to find this component, such as 
 import {showSlot} from './Product/SlotProduct';

11, now we click this link, it will call this commpnent and inserted it into router-outlet in index.html page
12, Slotproduct component is called, it will check this from app.module.ts, 
13, this slotproduct component is from product folder, slotproducts components
14, anglar parse this class showSLot , find out this component selector is <slot-prod> it will insert app.slot.html into router-outlet
15, app.slot.html contains data is from class  activeUserData: Promise<string[]>; 
16,  activeUserData: Promise<string[]> is used to accept the data from service
17, this service is injected into the constructor of the class
18, this servce is called when this compone tis called and class is contructued and return promise data 16 params is used to accept
19, this service is from angular http service 
20, http service in angular 2 is as below
var url="http://localhost/TimesheetAPIS/api/home/menus"; 
        let headers = new Headers({ 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Headers':'*','Access-Control-Allow-Methods': 'GET,PUT,POST,DELETE,PATCH,OPTIONS' });
        let options = new RequestOptions({ headers: headers });
      //return  this.http.get(url, options).map((res: Response) => (res.json()));
      return this.http.get(url)
      .map(res => res.json()) //must
      .toPromise()
     // .then(response =>response.json().data as string[])  
     //this should be mae in client side , not server side such as slotproduct.ts
     // prodService.getAlltimesheet().then((thisasr)=>console.log(thisasr));
      .catch(this.handleError);
22, url is the enabcors url from asp.net core in visual studio 2017
23, this web servce return promiise data to client via callback function
activeUserData: Promise<string[]>; //promise is important here, so we can return data as json
    constructor(private prodService: ProductSlotService) 
    { 
     prodService.getAlltimesheet().then
         (
            (ret)=>
            {
               this. activeUserData=  ret; //callback pass data to promise 
             }
        );
     }
this. activeUserData then is used in *ngFor="let item ofactiveUserData" ,  this is how http service consume data from sql server.
24, transfer data from one component to another isss a very specific in angular 2, you need to define a variable in higher tree , then send data from one lower tree to higher tree, pass this data to another variable in a varialbe in another lower component.


Angular Design


< body>
< app-root>
< header>
online timesheet report
< /header>
< nav>
home
product
timesheet
payment
< /nav>
< content>
<a routerlink="/slot">Slot</a>
<router-outlet></router-outlet>
< /content>
< footer>
2017@cc.com
< /footer>
< /app-root> 
< /body>
 






