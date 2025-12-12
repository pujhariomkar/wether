 @import "~ngx-toastr/toastr.css";
body {
  margin: 0;
}

/* You can add global styles to this file, and also import other style files */
/* html {
    overflow:   scroll;
  }
  ::-webkit-scrollbar {
      width: 0px;
      background: transparent; 
  } */
:root {
  /* --primary-color:#FFCE63;
  --secondary-color:#10316B;
  --primary-bg-color:#0B409C;
  --secondary-bg--color:#F2F7FF; */
  /* --unnamed-color-ffffff: #ffffff;
  --unnamed-color-2e65c3: #2e65c3;
  --unnamed-color-32599c: #32599c;
  --unnamed-color-0f141f: #0f141f;
  --unnamed-color-3e3e3e: #3e3e3e;
  --unnamed-color-557cbd: #557cbd;
  --unnamed-color-6b7280: #6b7280;
  --unnamed-color-1394EB:#1394EB; */

  /* Colors: */
  --unnamed-color-ffffff: #f6f6f6;
  --unnamed-color-6b7280: #6b7280;
  --unnamed-color-3e3e3e: #3e3e3e;
  --unnamed-color-0f141f: #0f141f;
  --unnamed-color-2e65c3: #089d92;
  --unnamed-color-32599c: #32599c;
  --unnamed-color-557cbd: #089d92;
  --unnamed-color-background: #b4e2e0;
  /* header icons */
  --unnamed-color-1394EB: #089d92;

  --unnamed-color-logo: #26dbcf;
  --unnamed-color-logosub: #089d92;
  --unnamed-color-transmit: #388cec;
  --unnamed-color-close: #16be7e;
  /* border #d9d0d0*/
  /* scheme1 */
  /* --unnamed-color-ffffff: #F6F5F5;
  --unnamed-color-6b7280: #6B7280;
  --unnamed-color-3e3e3e: #3E3E3E;
  --unnamed-color-0f141f: #0F141F;
  --unnamed-color-2e65c3: #1F3C88;
  --unnamed-color-32599c: #32599C;
  --unnamed-color-557cbd: #1F3C88;
  --unnamed-color-background: #d2ddfa;
  
  header icons
  --unnamed-color-1394EB:#070D59;
  
  --unnamed-color-logo:#1F3C88;
  --unnamed-color-logosub:#070D59;
  --unnamed-color-transmit:#EE6F57;
  --unnamed-color-close:#070D59;  */

  /* scheme2 */
  /* --unnamed-color-ffffff: #fff;
  --unnamed-color-6b7280: #6B7280;
  --unnamed-color-3e3e3e: #3E3E3E;
  --unnamed-color-0f141f: #0F141F;
  --unnamed-color-2e65c3: #1D2228;
  --unnamed-color-32599c: #32599C;
  --unnamed-color-557cbd: #1D2228;
  --unnamed-color-background: #E1E2E2;
  
  header icons
  --unnamed-color-1394EB:#E1E2E2;
  
  --unnamed-color-logo:#1D2228;
  --unnamed-color-logosub:#E1E2E2;
  --unnamed-color-transmit:#FB8122;
  --unnamed-color-close:#E1E2E2;   */

  /* scheme2 */
  /* --unnamed-color-ffffff: #fff;
  --unnamed-color-6b7280: #6B7280;
  --unnamed-color-3e3e3e: #3E3E3E;
  --unnamed-color-0f141f: #0F141F;
  --unnamed-color-2e65c3: #497174;
  --unnamed-color-32599c: #32599C;
  --unnamed-color-557cbd: #497174;
  --unnamed-color-background: #E1E2E2;
  
  header icons
  --unnamed-color-1394EB:#E1E2E2;
  
  --unnamed-color-logo:#73aaad;
  --unnamed-color-logosub:#E1E2E2;
  --unnamed-color-transmit:#EB6440;
  --unnamed-color-close:#E1E2E2;    */

  /* ogScheme */
  /* --unnamed-color-ffffff: #ffffff;
    --unnamed-color-6b7280: #6b7280;
    --unnamed-color-2e65c3: #2e65c3;
    --unnamed-color-32599c: #32599c;
    --unnamed-color-0f141f: #0f141f;
    --unnamed-color-3e3e3e: #3e3e3e;
    --unnamed-color-557cbd: #557cbd;
    --unnamed-color-557cbd: #557cbd;
    --unnamed-color-1394EB: #1394eb;
    --unnamed-color-logo: #00b5ef;
    --unnamed-color-logosub: #0d429d;
    --unnamed-color-transmit: #0085eb;
    --unnamed-color-close: #3dbc39;
    --unnamed-color-table-bg: #ecfbff; */

  /* Font/text values */
  --unnamed-font-family-roboto: Roboto;
  --unnamed-font-style-normal: normal;
  --unnamed-font-style-italic: italic;
  --unnamed-font-weight-300: 300px;
  --unnamed-font-weight-normal: normal;
  --unnamed-font-weight-medium: medium;
  --unnamed-font-size-11: 11px;
  --unnamed-font-size-14: 14px;
  --unnamed-font-size-16: 16px;
  --unnamed-font-size-18: 18px;
  --unnamed-font-size-20: 20px;
  --unnamed-font-size-22: 22px;
  --unnamed-font-size-32: 32px;
  --unnamed-character-spacing-0: 0px;
  --unnamed-line-spacing-13: 13px;
  --unnamed-line-spacing-19: 19px;
  --unnamed-line-spacing-22: 22px;
  --unnamed-line-spacing-24: 24px;
  --unnamed-line-spacing-29: 29px;
  --unnamed-line-spacing-32: 32px;
  --unnamed-line-spacing-38: 38px;
  --unnamed-line-spacing-54: 54px;
}

/* Character Styles */
.unnamed-character-style-1 {
  font-family: var(--unnamed-font-family-roboto);
  font-style: var(--unnamed-font-style-normal);
  font-weight: var(--unnamed-font-weight-normal);
  font-size: var(--unnamed-font-size-14);
  line-height: var(--unnamed-line-spacing-19);
  letter-spacing: var(--unnamed-character-spacing-0);
  color: var(--unnamed-color-557cbd);
}
.unnamed-character-style-2 {
  font-family: var(--unnamed-font-family-roboto);
  font-style: var(--unnamed-font-style-normal);
  font-weight: var(--unnamed-font-weight-normal);
  font-size: var(--unnamed-font-size-14);
  line-height: var(--unnamed-line-spacing-32);
  letter-spacing: var(--unnamed-character-spacing-0);
  color: var(--unnamed-color-3e3e3e);
}
.unnamed-character-style-3 {
  font-family: var(--unnamed-font-family-roboto);
  font-style: var(--unnamed-font-style-normal);
  font-weight: var(--unnamed-font-weight-normal);
  font-size: var(--unnamed-font-size-16);
  line-height: var(--unnamed-line-spacing-19);
  letter-spacing: var(--unnamed-character-spacing-0);
  color: var(--unnamed-color-ffffff);
}
.unnamed-character-style-4 {
  font-family: var(--unnamed-font-family-roboto);
  font-style: var(--unnamed-font-style-normal);
  font-weight: var(--unnamed-font-weight-normal);
  font-size: var(--unnamed-font-size-22);
  line-height: var(--unnamed-line-spacing-29);
  letter-spacing: var(--unnamed-character-spacing-0);
  color: var(--unnamed-color-0f141f);
}
.unnamed-character-style-5 {
  font-family: var(--unnamed-font-family-roboto);
  font-style: var(--unnamed-font-style-normal);
  font-weight: var(--unnamed-font-weight-300);
  font-size: var(--unnamed-font-size-32);
  line-height: var(--unnamed-line-spacing-38);
  letter-spacing: var(--unnamed-character-spacing-0);
  color: var(--unnamed-color-0f141f);
}
.unnamed-character-style-6 {
  font-family: var(--unnamed-font-family-roboto);
  font-style: var(--unnamed-font-style-normal);
  font-weight: var(--unnamed-font-weight-medium);
  font-size: var(--unnamed-font-size-16);
  line-height: var(--unnamed-line-spacing-19);
  letter-spacing: var(--unnamed-character-spacing-0);
  color: var(--unnamed-color-32599c);
}
.unnamed-character-style-7 {
  font-family: var(--unnamed-font-family-roboto);
  font-style: var(--unnamed-font-style-normal);
  font-weight: var(--unnamed-font-weight-medium);
  font-size: var(--unnamed-font-size-11);
  line-height: var(--unnamed-line-spacing-13);
  letter-spacing: var(--unnamed-character-spacing-0);
  color: var(--unnamed-color-ffffff);
}
.unnamed-character-style-8 {
  font-family: var(--unnamed-font-family-roboto);
  font-style: var(--unnamed-font-style-normal);
  font-weight: var(--unnamed-font-weight-medium);
  font-size: var(--unnamed-font-size-18);
  line-height: var(--unnamed-line-spacing-22);
  letter-spacing: var(--unnamed-character-spacing-0);
  color: var(--unnamed-color-2e65c3);
}
.unnamed-character-style-9 {
  font-family: var(--unnamed-font-family-roboto);
  font-style: var(--unnamed-font-style-italic);
  font-weight: var(--unnamed-font-weight-normal);
  font-size: var(--unnamed-font-size-20);
  line-height: var(--unnamed-line-spacing-24);
  letter-spacing: var(--unnamed-character-spacing-0);
  color: var(--unnamed-color-ffffff);
}
.unnamed-character-style-10 {
  font-family: var(--unnamed-font-family-roboto);
  font-style: var(--unnamed-font-style-normal);
  font-weight: var(--unnamed-font-weight-normal);
  font-size: var(--unnamed-font-size-18);
  line-height: var(--unnamed-line-spacing-54);
  letter-spacing: var(--unnamed-character-spacing-0);
  color: var(--unnamed-color-6b7280);
}
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  margin: 0;
}
select::-ms-expand {
  display: none;
}
#toast-container > div {
  opacity: 1;
}
.toast:not(.show) {
  display: block;
}
