# sudo0x18 Level1

Very easy and beginner friendly crackme. Reverse Engineer and get the password.

```c
undefined8 main(void) {
  int iVar1;
  undefined local_48 [64];
  
  printf("Welcome to Easy Crack Me");
  printf("What is the Secret ?");
  __isoc99_scanf(&DAT_00102032,local_48);
  iVar1 = checkPass(local_48);
  if (iVar1 == 0) {
    printf("Better luck next time. :(");
  }
  else {
    printf("You are correct :)");
  }
  return 0;
}
```

```c
char checkPass(char *param_1) {
  char cVar1;
  
  if (*param_1 == 's') {
    cVar1 = param_1[1];
    if ((((cVar1 == 'u') && (cVar1 = param_1[2], cVar1 == 'd')) &&
        (cVar1 = param_1[3], cVar1 == 'o')) &&
       (((cVar1 = param_1[4], cVar1 == '0' && (cVar1 = param_1[5], cVar1 == 'x')) &&
        ((cVar1 = param_1[6], cVar1 == '1' && (cVar1 = param_1[7], cVar1 == '8')))))) {
      cVar1 = '\x01';
    }
  }
  else {
    cVar1 = '\0';
  }
  return cVar1;
}
```

### Beautified

```c
int main(void) {
  int result_val;
  char user_input [64];
  
  printf("Welcome to Easy Crack Me");
  printf("What is the Secret ?");
  __isoc99_scanf(&DAT_00102032,user_input);
  result_val = checkPass(user_input);
  if (result_val == 0) {
    printf("Better luck next time. :(");
  }
  else {
    printf("You are correct :)");
  }
  return 0;
}
```

```c
char checkPass(char *password) {
  char chr;
  
  if (*password == 's') {
    chr = password[1];
    if ((((chr == 'u') && (chr = password[2], chr == 'd')) && (chr = password[3], chr == 'o')) &&
       (((chr = password[4], chr == '0' && (chr = password[5], chr == 'x')) &&
        ((chr = password[6], chr == '1' && (chr = password[7], chr == '8')))))) {
      chr = '\x01';
    }
  }
  else {
    chr = '\0';
  }
  return chr;
}
```

```
sudo0x18
```