# Channelestimation

% PARAMETERS FOR TWO DIFFERENT CHANNELS OBJECTS THAT IS CONSIDERED
sampleRate500KHz = 500e3;
sampleRate20KHz  = 20e3;
maxDopplerShift  = [100 200 300 400 500]; 
delayVector = [1e-3  2e-3  3e-3  4e-3 5e-3]
gainVector  =[30 10 0 -3 -6] 
kfactor=10;
specDopplerShift=100;
% CREATION OF OBJECTS OF TWO DIFFERENT CHANNELS RAYLEIGH AND RICIAN CHANNEL
% MODEL
M=4; 
A=[4 16 64 256];
numBits = log2(M); % Number of bits per symbol
% raybit = zeros(size(raydemodtx, 1), numBits * numel(delayVector));
for i=1:length(delayVector)
    rayChan = comm.RayleighChannel(SampleRate=sampleRate500KHz,PathDelays=delayVector(i),AveragePathGains=gainVector(i),MaximumDopplerShift=maxDopplerShift(i),RandomStream="mt19937ar with seed",Seed=10,PathGainsOutputPort=true);
    ricChan = comm.RicianChannel(SampleRate=sampleRate500KHz,PathDelays=delayVector(i),AveragePathGains=gainVector(i),KFactor=kfactor,DirectPathDopplerShift=specDopplerShift,MaximumDopplerShift=maxDopplerShift(i),RandomStream="mt19937ar with seed",Seed=100,PathGainsOutputPort=true);
    % COMMUNICATION BLOCK

     % % MESSAGE BLOCK
     % n=2000;
     % msg(i)=randi([0 1],n,1);
     n=1024;
     msg( :,i)=randi([0 1],n,1);
    qpsk_modsig( :,i)=pskmod(msg( :,i),M,InputType="bit");
    qam4_modsig(:,i)=qammod(msg(:,i),A(1),InputType="bit");
    qam16_modsig(:,i)=qammod(msg(:,i),A(2),InputType="bit");
    qam256_modsig(:,i)=qammod(msg(:,i),A(4),InputType="bit");

     for j=1:n:1e5
         % msg passed through channel
         a( :,i)=rayChan(qpsk_modsig( :,i));
         b( :,i)=ricChan(qpsk_modsig(:,i));
         c(:,i)=rayChan(qam4_modsig(:,i));
         d(:,i)=ricChan(qam4_modsig(:,i));
         e(:,i)=rayChan(qam16_modsig(:,i));
         f(:,i)=ricChan(qam16_modsig(:,i));
         g(:,i)=rayChan(qam256_modsig(:,i));
         h(:,i)=ricChan(qam256_modsig(:,i));

     end
     % transmitted signals are demodulated
    qpsk_raydemodrx( :,i)=pskdemod(a( :,i),M,OutputType="bit");
    qpsk_ridemodrx( :,i)=pskdemod(b( :,i),M,OutputType="bit");
    qam4_raydemodsig(:,i)=qamdemod(c(:,i),A(1),OutputType="bit");
    qam4_ridemodsig(:,i)=qamdemod(d(:,i),A(1),OutputType="bit");
    qam16_raydemodsig(:,i)=qamdemod(e(:,i),A(2),OutputType="bit");
    qam16_ridemodsig(:,i)=qamdemod(f(:,i),A(2),OutputType="bit");
    qam256_raydemodsig(:,i)=qamdemod(g(:,i),A(4),OutputType="bit");
    qam256_ridemodsig(:,i)=qamdemod(h(:,i),A(4),OutputType="bit");

     % calculation of errors in each path
         biterrray_qpsk( :,i)=biterr(msg( :,i),qpsk_raydemodrx( :,i));
         biterrri_qpsk( :,i)=biterr(msg( :,i),qpsk_ridemodrx( :,i));
         biterrray_qam4( :,i)=biterr(msg( :,i),qam4_raydemodsig(:,i));
         biterrri_qam4( :,i)=biterr(msg( :,i),qam4_ridemodsig(:,i));
         biterrray_qam16( :,i)=biterr(msg( :,i),qam16_raydemodsig(:,i));
         biterrri_qam16( :,i)=biterr(msg( :,i),qam16_ridemodsig(:,i));
         biterrray_qam256( :,i)=biterr(msg( :,i),qam256_raydemodsig(:,i));
         biterrri_qam256( :,i)=biterr(msg( :,i),qam256_ridemodsig(:,i));
    
     % integer to binary conversion 
     % raybit( :,i)=de2bi(raydemodtx( :,i));
    
     % % taking magnitude of rounded complex values
     % rayabstx( :,i)=round(abs(raycomptx( :,i)));
     % riabstx( :,i)=round(abs(ricomptx( :,i)));
     % 
     % % conversion of integers to bits
     % raybittx( :,i)=int2bit(rayabstx( :,i),M/2);
     % ribittx( :,i)=int2bit(riabstx( :,i),M/2);
     % % calculationn of bit errors
     % for c=1:500:1e5
     %     numbits( :,i)=numbits+i;
     %     biterrray( :,i)=biterr(msg( :,i),raybittx( :,i));
     %     biterrri( :,i)=biterr(msg( :,i),ribittx( :,i));
     % end
end
directory='C:\Users\Benie Jaison\Desktop\Channelesti';
qpsk_mod_dataset_msg='msg.csv';

% MODULATION QPSK DATASET
qpsk_mod_dataset_rx_ray='qpsk_raydemodrx.csv';
qpsk_mod_dataset_rx_ri='qpsk_ridemodrx.csv';
f_msg=fullfile(directory,qpsk_mod_dataset_msg);
f_qpsk_ray=fullfile(directory,qpsk_mod_dataset_rx_ray);
f_qpsk_ri=fullfile(directory,qpsk_mod_dataset_rx_ri);
writematrix(msg,f_msg);
writematrix(qpsk_raydemodrx,f_qpsk_ray);
writematrix(qpsk_ridemodrx,f_qpsk_ri);

%  Modulation QAM4 dataset
qam4_mod_dataset_rx_ray='qam4_raydemodsig.csv';
qam4_mod_dataset_rx_ri='qam4_ridemodsig.csv';
f_qam4_ray=fullfile(directory,qam4_mod_dataset_rx_ray);
f_qam4_ri=fullfile(directory,qam4_mod_dataset_rx_ri);
writematrix(qam4_raydemodsig,f_qam4_ray);
writematrix(qam4_ridemodsig,f_qam4_ri);

% MODULATION QAM16 DATASET
qam16_mod_dataset_rx_ray='qam16_raydemodsig.csv';
qam16_mod_dataset_rx_ri='qam16_ridemodrx.csv';
f_qam16_ray=fullfile(directory,qam16_mod_dataset_rx_ray);
f_qam16_ri=fullfile(directory,qam16_mod_dataset_rx_ri);
writematrix(qam16_raydemodsig,f_qam16_ray);
writematrix(qam16_ridemodsig,f_qam16_ri);

% MODULATION QAM256 DATASET
qam256_mod_dataset_rx_ray='qam256_raydemodsig.csv';
qam256_mod_dataset_rx_ri='qam256_ridemodsig.csv';
f_qam256_ray=fullfile(directory,qam256_mod_dataset_rx_ray);
f_qam256_ri=fullfile(directory,qam256_mod_dataset_rx_ri);
writematrix(qam256_raydemodsig,f_qam256_ray);
writematrix(qam256_ridemodsig,f_qam256_ri);

% MODULATION QAM256 DATASET
qam256_mod_dataset_rx_ray='qam256_raydemodsig.csv';
qam256_mod_dataset_rx_ri='qam256_ridemodsig.csv';
f_qam256_ray=fullfile(directory,qam256_mod_dataset_rx_ray);
f_qam256_ri=fullfile(directory,qam256_mod_dataset_rx_ri);
writematrix(qam256_raydemodsig,f_qam256_ray);
writematrix(qam256_ridemodsig,f_qam256_ri);
