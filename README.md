# Developing Angluar 2 Front End Web Application with asp.net Web API 
<pre>

<b>Introduction</b>

I have developed an Asp.Net MVC 5 web portal to manage the online timesheet, inventory, and wage payment for my family business. Now, 
It is my time to develop an angular 2 dashboard to manage the timesheet, cash flow, and wage payment for this timesheet system. 
I used angular 2, Node js, NPM , visual studio code, IIS web server, and asp.net Web API2 in visual studio 2017 to develop 
the report system based on the portal's backend.

<b>Get Started</b>

  A, Asp.net Web API

        1, develop a new asp.net core Web API project in VS 2017
        2, create some CRUD APIs to be used.
        3, install IIS asp.net core hosting in window component as below
        https://go.microsoft.com/fwlink/?linkid=844461
        4, restart IIS web server 
        5, publish this asp.net core web APIs to a folder such as publishoutput
        6, create a virtual directory in IIS web server
        7, convert this virtual directory to a web application
        8, open the api link to get json data such as 
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

<b>Angular 2 Code in place</b>

  Angular is only doing one thing for us, that is to create HTML element and generate a DOM tree as HTMLin angular way.
  HTML page is composed of basic html, head, and body tags. Inside body tag we can add any elements to generate a page. 
  Angular is doing in its own way to custom tags inside body tags such as <app-root></app-root> and then bring all child 
  components into page to create DOM tree. 

  Our Angular front deveopment is starting from here

      1, create a root app component to insert a top element app-root next to body tag.
      2, after root element is installed inside body tag, we need to install other html elements inside this root angular component
      3, we can add event and style to different elements. 
      4, HTML elements want to present business data in web page, angular invents http servce to get data from local and remote data              resources such as files and databases. 
      5, http service is the only way angular can communicate with database on the web.
      6, typescript in angular then can consume this service via component object
      7, typescript uses constructor to inject the dependency into javascript
      8, this then can be used to push data from dependency into component to present data in page.
      9, element is from component and is composed with a tree structure, top branch can see lower branch, lower branch can emit data to          higher branch
      10,......

 <b>Root component</b>

  Now we understand that root component is a critical component we can get started angualr from here, developing root component needs to   build all necessary enironment to generate this root component inside html body tag. so what the enviorment for angular 2 is ?
 
   1, needs a html page such as index.html to install this root element 
   
    < body>
      < app-root>< /app-root>
    < /body>

  2, when browser calls index.html page, it needs to parse app-root selector.

  3, call app-root component, html needs to find out this tag from app.module.ts file, where we import all components including the top      level component such as 
    
    import { AppComponent } from './app.component';

  4, browser then goes to app.component.ts file to find out the AppComonent class
     
     export class AppComponent{
        title = 'Tea Journal Time sheet';
      }

  5, this class has a component as an attribute, so we can say appcomponent is a top-component

  6, component includes
     
     @Component({
        selector: 'app-root',
        templateUrl: './app.component.html',
        styleUrls: ['./app.component.css']
      })
      
      it means in index.html page, browser parse app-root selector and comes here to open tempate url and add style in

  7, so this templateurl is a div element only, that will be embedded into index.html page
  
      such as 
      < div class="col-sm-12">
        < h1>
          {{title}}
        < /h1>
        < nav>
          < a routerLink="/slot" routerLinkActive="active">Slot< /a>
         < /nav>
      < div class=col-sm-4>
        < router-outlet></router-outlet>
      < /div>
      < /div>
      
  8, app-root selector will insert this templateurl into index.html page,  it finds the title value from appcomponent class and embed        the content into the <router - outlet> to host routerlink content

  9, routerlink attributes should be read from app.module.ts top level module to see which compnent is linked and clicked, then this          componet content can be inserted into router-outlet

  10, /slot link will call slotproduct.ts component , so it goes to app.module.ts to find this component, such as 
    import {showSlot} from './Product/SlotProduct';

  11, click this link, it will call this commpnent and insert it into router-outlet in index.html page
 
   12, slotproduct component from product folder is called, it will check this from app.module.ts, 

   13, anglar parses this class showSLot ,finds out this component selector, <slot-prod>,  it will insert app.slot.html into router-            outlet

   14, app.slot.html contains data that is from class activeUserData: Promise<string[]>; 

   16, activeUserData: Promise<string[]> is used to accept the data from service

   17, this service is injected into the constructor of the class

   18, this angular http service is called when this componet is called and the related class is constructued. class returns promise             data 

   19, http service in angular 2 is as below

    var url="http://localhost/TimesheetAPIS/api/home/menus"; 
        let headers = new Headers({ 'Access-Control-Allow-Origin': '*', 'Access-Control-Allow-Headers':'*','Access-Control-Allow-               Methods': 'GET,PUT,POST,DELETE,PATCH,OPTIONS' });
        let options = new RequestOptions({ headers: headers });
        return this.http.get(url)
          .map(res => res.json()) //must
          .toPromise()
          .catch(this.handleError);

   url is the enabcors url from asp.net core in visual studio 2017

  20, this web servce returns promiise data to client via callback function

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
         
        this.activeUserData then is used in *ngFor="let item ofactiveUserData" to present the data.

    21, transfer data from one component to another is a very topic in angular 2, you need to define a variable in higher level of the tree , then you can send data from one lower tree to higher tree, pass this data to another variable in another lower component.
 
<b>Angular Design</b>

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
  
<b>Result</b>

  1, After we put all components together in app.component.ts, we get the final DOM tree as below

<img src="https://github.com/davidlizhonghuang/NG2CLIAspNetWebAPI/blob/master/as3.png">

  we can see the DOM tree is the expected result we designed and developed. 

  2, The page appearance is as below

  <img src="https://github.com/davidlizhonghuang/NG2CLIAspNetWebAPI/blob/master/as4.png">

<b>Pagination example</b>

I built up an action in MVC controller to return a user list from backend as a json. I configure web.config to enable CORS. now, in angular 2 , I built up a tealist component to list vip users with pagination compnent as example image below 

<img src="https://github.com/davidlizhonghuang/NG2CLIAspNetWebAPI/blob/master/pagg.png">

The development steps include

  1, generate a new method to return a list of users from SQL server database from repository class
  2, create an action in MVC controller to return a json result of a lst of users
  3, create a new component with command: 
        ng g component tealist
  4,create a new data service teadataservice.ts as code below
    @Injectable()
    export class TeaUserService{
        constructor(private http: Http) { }
        getAllUserList()
        {
            const url = 'http://www.xxx.com/xxx/xxxMemberJson'; 
            const headers = new Headers(
              { 'Access-Control-Allow-Origin': '*', 
              'Access-Control-Allow-Headers':'*',
              'Access-Control-Allow-Methods': 'GET,PUT,POST,DELETE,PATCH,OPTIONS' 
            });
            const options = new RequestOptions({ headers: headers });
          return this.http.get(url)
          .map(res => res.json())
          .toPromise()
          .catch(this.handleError);
        }
        private handleError(error: any): Promise<any> {
            console.error('An error occurred', error); 
            return Promise.reject(error.message || error);
          }
    }
    5, register this data service in app.module.ts
    6, call teadataservice.ts in tealist component, the code is as below
   
      constructor(private userdataService: TeaUserService) {
          this.activeUserData = null;
          userdataService.getAllUserList().then
           (
              (ret) => 
              {
                 this. activeUserData =  ret;
                 console.log(this.activeUserData);
              }
          );
      }
      CDatee(ds: string): Date {
        return  new Date( 
            parseInt (ds.replace('/Date(', ''))
          );
      }  
   
    7, loop data in the list html page as code below
    
    ...
     <tr *ngFor="let item of activeUserData.UserList  | paginate: { itemsPerPage: 10, currentPage: p }">
    <td>{{item.Companyname}}</td>
    <td>
    {{CDatee(item.CreatedDate).toISOString().substring(0, 10)}}
    </td>
    <td>{{item.Username}}</td>
    <td>{{item.OfficeTelphone}}</td>
    <td>{{item.Email}}</td>
    ... 
     <pagination-controls (pageChange)="p = $event"></pagination-controls>
    ...
   
    8, load the page, we get the expected result.
   </pre>



