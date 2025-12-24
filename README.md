import { Injectable } from '@angular/core';
import { HttpClient, HttpResponse } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ApiService {

  constructor(private http: HttpClient) {}

  getData(): Observable<HttpResponse<any>> {
    const url = 'YOUR_API_URL_HERE';

    return this.http.get<any>(url, {
      observe: 'response'
    });
  }
}
