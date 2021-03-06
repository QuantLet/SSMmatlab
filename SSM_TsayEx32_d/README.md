[<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/banner.png" width="888" alt="Visit QuantNet">](http://quantlet.de/)

## [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/qloqo.png" alt="Visit QuantNet">](http://quantlet.de/) **SSM_TsayEx32_d** [<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/QN2.png" width="60" alt="Visit QuantNet 2.0">](http://quantlet.de/)

```yaml

Name of QuantLet: SSM_TsayEx32_d

Published in: Linear Time Series With MATLAB and Octave

Description: 'An MA(1) model is identified and estimated for the three series of Example 3.2 in Tsay (2014).'

Keywords: time-series, VARMA model, identification, estimation, cross-correlation matrices

Author: Víctor Gómez

Submitted: Thu, December 20 2018 by Víctor Gómez

```

![Picture1](USIPItbphp.png)

### MATLAB Code
```matlab

%script file for example3.2 in Tsay (2014)
%

data = load(fullfile('data', 'm-dec15678-6111.dat'));
x = log(data(:, 2:6)+1) * 100;
% size(x)
rtn = x(:, [2, 5]);
tdx = (1:612) / 12 + 1961;

subplot(2, 1, 1)
plot(tdx, rtn(:, 1))
xlabel('year');
ylabel('d5');
axis('tight');
subplot(2, 1, 2)
plot(tdx, rtn(:, 2))
xlabel('year');
ylabel('d8');
axis('tight');
disp('press any key to continue')
pause
close all

disp('cross correlation matrices')
%ccm matrices
lag = 6;
ic = 1;
str = mautcov(rtn, lag, ic);
disp('Correlation matrix at lag 0:')
disp(str.r0)

disp('identify a VARMA(p,q) model for the series')
disp('press any key to continue')
pause
%identify a VARMA(p,q) model for the series
maxlag = 6;
minlag = 0;
prt = 1;
x = [];
seas = 1;
[lagsopt, ~] = lratiopqr(rtn, x, seas, maxlag, minlag, prt);
disp(' ')
disp('Estimated orders in VARMAX(p,q,r):  ')
disp(lagsopt)
disp('press any key to continue')
pause

%estimate an MA(1) model using the Hannan-Rissanen method
disp(' ')
disp('estimate an MA(1) model using the Hannan-Rissanen method: ')
disp('press any key to continue')
pause

freq = 1;
xx = [];
hr3 = 0;
finv2 = 1;
[strv, ferror] = estvarmaxpqrPQR(rtn, xx, freq, [0, 1, 0], [0, 0, 0], hr3, finv2);

disp(' ');
disp('***** Estimated  MA(1)  Model  *****');
disp(' ');
clear in
in.fid = 1;
in.fmt = char('%12.4f');
tit = 'mu:';
mprintar(strv.mus3', in, tit);
disp('press any key to continue')
pause
tit = 'th:';
strt = 1;
mprintar(strv.thetas3(:, :, 2), in, tit, strt);
disp('press any key to continue')
pause

tit = 'tv-th:';
strt = 1;
mprintar(strv.thetatv3(:, :, 2), in, tit, strt);
disp('press any key to continue')
pause
tit = 'Sigma:';
mprintar(strv.sigmar3, in, tit);
disp('press any key to continue')
pause

%estimate the model using the conditional method
[xvfc, strc, ferrorc] = mconestim(rtn, xx, strv);

disp(' ');
disp('***** Estimated Model using the conditional method  *****');
disp(' ');
clear in
in.fid = 1;
tit = 'th:';
strt = 1;
mprintar(strc.thetascon(:, :, 2), in, tit, strt);

disp(' ');
tit = 'Sigma:';
mprintar(strc.sigmarcon, in, tit);

disp(' ')
disp('t-values: ')
in.fmt = char('%12.4f');
disp(' ');
tit = 'tv-th:';
strt = 1;
mprintar(strc.thetatvcon(:, :, 2), in, tit, strt);

disp('press any key to continue')
pause

lag = 24;
ic = 1;
nr = strc.nparm;
disp(' ')
disp('******** Conditional Residuals:     ********');
str = mautcov(strc.residcon, lag, ic, nr);
disp('Correlation matrix at lag 0:')
disp(str.r0)
disp('Q statistics:')
disp(str.qstat)

disp('p-values of Q statistics:')
disp(str.pval)
[m, n] = size(str.pval);
t = 1:m;
plot(t, str.pval, t, 0.05*ones(1, m))
legend('p-values of Q statistics:')
pause
close all

```

automatically created on 2019-02-11