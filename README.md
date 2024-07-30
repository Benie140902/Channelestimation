# Channelestimation
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
