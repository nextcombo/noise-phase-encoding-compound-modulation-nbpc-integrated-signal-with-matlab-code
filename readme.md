# Noise-Phase Encoding Compound Modulation (NBPC) Integrated Signal [with MATLAB Code]

In electronic warfare systems, on the one hand, phase-encoded signals are currently a commonly used type of pulse radar compression signals that can accurately detect and locate targets. They have broad bandwidth and wide time-bandwidth characteristics, with sharp autocorrelation properties and a "pincushion"-like fuzzy function. On the other hand, noise always plays an excellent role as an interference source. Typical interference signal models include Gaussian white noise, which has a uniform power spectrum and Gaussian statistical characteristics, offering the best suppressive interference effects. Based on the dual characteristics of shared signals, it is considered to combine the above two signals through compound modulation to produce a novel composite signal, namely, noise-phase encoded compound modulated signal. This signal not only possesses the radar characteristics of phase encoding but also the suppressive interference characteristics of white noise, enabling integrated system energy sharing.
\n        \n  
\n        \n  
\n        \n  
\n        \n  
\n        \n  
Partial code display: clc,clear,close all;\n\n\n%% NBPC ambiguity figure\nload NBPC.mat\nN = size(s_nbpc,2);%NBPC sequence length\ntau = N;\n\n\nsamp_num = N * 10;\nn = ceil(log(samp_num)/log(2));\nnfft = 2^n;\n\n\nU(1:nfft) = 0;\nj = 0;\nfor index = 1:10:samp_num \n    j = j+1;\n    U(index:index+10-1) = s_nbpc(j);\nend\n% Calculate U(f)\nV = U;\ndelay = linspace(-tau,tau,nfft);\nfreq_del = 12/tau/100;%-6/tau to 6/tau divided into 100 parts\nvfft = fft(V,nfft);\n\n\nj = 0;\nfor freq = -6/tau:freq_del:6/tau % Frequency axis takes 101 points\n    j = j+1;\n    exf = exp(sqrt(-1)*2.*pi.*freq.*delay);%e^(j2*pi*fd*delay)\n    u_times_exf = U .* exf;%s(t)*e^(j2*pi*fd*delay), prepare for frequency shift\n    ufft = fft(u_times_exf,nfft);%S(f-fd)\n    prob = ufft.*conj(vfft);% S(f-fd)* conjugate [S(f)]\n    ambig(:,j) = fftshift(abs(ifft(prob))');% Fuzzy function = |Integral sign Conjugate[S(f)]*S(f-fd)*e^(-j*2*pi*f*tau)|^2\nend\n\n\n%% NLFM Fuzzy Function Diagram (Fuzzy Diagram + Distortion Recognition Diagram + Velocity Recognition Diagram)\nfreq = -6/tau:freq_del:6/tau;\nFreq = 10*(1e+6)*freq/(6/tau);\ndelay = 1.3*linspace(-N,N,nfft)*(1e-6)/N;\n\n\nfigure(1)\nmesh(Freq,delay,ambig./max(max(ambig)));% Innermost max: Find the maximum value in each column; Outermost max: Find the maximum value in that row.\ntitle('NBPCSignal blunting function graph');\naxis tight\nxlabel('frequency/MHz');ylabel('delay/\mus');zlabel('ambiguity function');\n\n\nfigure(2)\n%value = 10*N;\nplot(delay,ambig(:,51)/max(ambig(:,51)));\nxlabel('delay/us');ylabel('normalized ambiguity cut for f=0');\ntitle('NBPCSignal Distance Resolution Function Graph');\ngrid on\naxis tight\n\n\nfigure(3)\nplot(Freq,ambig(4096,:)/(max(ambig(4096,:))));\nxlabel('freqency/MHz');ylabel('normalized ambiguity cut for \tau=0');\ntitle('NBPCSignal speed blur function graph');\ngrid on\naxis tight\n  

## Images

![Noise-Phase Coded Complex Modulation (NBPC) Integrated Signal [with MATLAB code]](01.jpg)
![Noise-Phase Coded Complex Modulation (NBPC) Integrated Signal [with MATLAB code]](02.jpg)
![Noise-Phase Coded Complex Modulation (NBPC) Integrated Signal [with MATLAB code]](03.jpg)
![Noise-Phase Coded Complex Modulation (NBPC) Integrated Signal [with MATLAB code]](04.jpg)
![Noise-Phase Coded Complex Modulation (NBPC) Integrated Signal [with MATLAB code]](05.jpg)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
