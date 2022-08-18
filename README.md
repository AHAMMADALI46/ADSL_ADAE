# ADSL_ADAE

data vs1 (keep=usubjid vstestcd vsorres);
set vs;
where vsblfl="Y" and vstestcd in ( "Weight" "BMI") and VSORRES ne ' ';
run;

data ht (keep=usubjid vstestcd vsorres);
set vs;
where vstestcd= "Height" and vsorres ne ' ';
run;

data vs3;
set ht vs1;
run;

Proc sort data=vs3;
by usubjid;
run;

Proc transpose data=vs3 out=vsb1 (rename=(height=heightb1 weight=weightb1 bmi=bmifl));
vat vsorres;
id vstestcd;
by usubjid;
run;

/*Merge VSb1 to dm5*/
proc sort data=dm5;
by usubjid;
run;

proc sort data=vsb1;
by usubjid;
run;

data dm6;
merge dm5(in=a) vsb1 (drop=_name_ _label_);
by usubjid;
if a;
run;

data mh1 (keep=usubjid ASTHMAFL);
set mh;
where mhterm="ASTHMA" and Mhoccur ne ' ';
rename mhoccur=ASTHMAFL;
label ASTHMAFL="Asthma flag ";
run;
/*Merge*/
proc sort data=dm6;
by usubjid;
run;

proc sort data=mh1;
by usubjid;
run;

data dm7;
merge dm6(in=a) mh1;
by usubjid;
if a;
run;      
