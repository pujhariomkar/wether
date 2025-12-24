
import { HttpClient, HttpHeaders, HttpResponse } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class ApiService {

  constructor(private http: HttpClient) {}

  postAdeshDetails(url: string, headers: HttpHeaders, data: any)
    : Observable<HttpResponse<any>> {

    return this.http.post<any>(
      url,
      data,
      {
        headers: headers,
        observe: 'response'   // ⭐ REQUIRED
      }
    );
  }
}
