import { BrowserModule } from "@angular/platform-browser";
import { NgModule } from "@angular/core";
import { FormsModule, ReactiveFormsModule } from "@angular/forms";
import { AppComponent } from "./app.component";
import { HeaderComponent } from "./layout/header/header.component";
import { SidebarComponent } from "./layout/sidebar/sidebar.component";
import { DashboardComponent } from "./layout/dashboard/dashboard.component";
import { ProfilecardComponent } from "./components/profilecard/profilecard.component";
import { AppRoutingModule } from "./app-routing.module";
import { HomeComponent } from "./pages/home/home.component";
import { FavouritecardComponent } from "./components/favouritecard/favouritecard.component";
import { LocalstorageService } from "./services/localstorage/localstorage.service";
import { HTTP_INTERCEPTORS, HttpClientModule } from "../../node_modules/@angular/common/http";

import { PageNotFoundComponent } from "./pages/page-not-found/page-not-found.component";

import { SpinnerModule } from "ng-spinner";
import { ErrorComponent } from "./pages/error/error.component";


import { PaginatorComponent } from "./components/paginator/paginator.component";


import { ModalComponent } from "./components/modal/modal.component";

import { SuccessCardComponent } from "./components/success-card/success-card.component";

import { DatepickerComponent } from "./components/datepicker/datepicker.component";
import { BnNgIdleService } from "bn-ng-idle";
import { HashLocationStrategy, LocationStrategy } from '@angular/common';

import {
  ToastrModule,
  ToastNoAnimation,
  ToastNoAnimationModule,
} from "ngx-toastr";
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { ToastContainerComponent } from './components/toast-container/toast-container.component';
import { PendingcardComponent } from './components/pendingcard/pendingcard.component';

import { SsoComponent } from './pages/sso/sso.component';
import { InputfieldsComponent } from './components/inputfields/inputfields.component';
import { CopypasteDirective } from './directive/copypaste.directive';
import { LandingpageComponent } from "./pages/landingpage/landingpage/landingpage.component";
import { LogoutComponent } from "./pages/logout/logout.component";
import { FlaggyComponent } from './pages/flaggy/flaggy.component';
import { FilterPipe } from './filter.pipe';




@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    SidebarComponent,
    DashboardComponent,
    ProfilecardComponent,
    HomeComponent,
    FavouritecardComponent,

    PageNotFoundComponent,

    ErrorComponent,

    PaginatorComponent,

    ModalComponent,

    SuccessCardComponent,

    DatepickerComponent,
    ToastContainerComponent,
    PendingcardComponent,

    SsoComponent,
    InputfieldsComponent,
    CopypasteDirective,
    LandingpageComponent,


    LogoutComponent,


    FlaggyComponent,


    FilterPipe,


    
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    ReactiveFormsModule,
    HttpClientModule,
    FormsModule,
    SpinnerModule,
    ToastrModule.forRoot({
      
      timeOut: 3000,
      positionClass: "toast-bottom-left",
      maxOpened: 1, // Maximum number of visible toasts
      autoDismiss: true 

    }),

    
    BrowserAnimationsModule
  ],

  providers: [LocalstorageService, HttpClientModule, BnNgIdleService, { provide: LocationStrategy, useClass: HashLocationStrategy }],
  bootstrap: [AppComponent],
})
export class AppModule { }

