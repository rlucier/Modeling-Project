Modeling-Project
====================

BMED 4784 Project
Problem 1 (Steady-State)

function steadyState()
    
    gBar_K = 36; 
    gBar_Na = 120; 
    gBar_L= 0.3;
    e_K = -12; 
    e_Na = 115; 
    e_L = 10.6;
    v = 0;
    c = 1;
    
    a_m = 0.1*((25-v)/(exp((25-v)/10)-1));
    b_m = 4*exp(-v/18);
    a_n = 0.01*((10-v)/(exp((10-v)/10)-1));
    b_n = 0.125*exp(-v/80);
    a_h = 0.07*exp(-v/20);
    b_h = 1/(exp((30-v)/10)+1);

    m(1) = a_m/(a_m+b_m);
    n(1) = a_n/(a_n+b_n);
    h(1) = a_h/(a_h+b_h);

    endTime = 100;
    timeStep = 0.1;
    t = 0:timeStep:endTime;
    
    injectedCurrent = 0;
   
    for i = 1:numel(t)-1
   
        a_m(i) = 0.1*((25-v(i))/(exp((25-v(i))/10)-1));
        b_m(i) = 4*exp(-v(i)/18);
        a_n(i) = 0.01*((10-v(i))/(exp((10-v(i))/10)-1) );
        b_n(i) = 0.125*exp(-v(i)/80);
        a_h(i) = 0.07*exp(-v(i)/20);
        b_h(i) = 1/(exp((30-v(i))/10)+1);
   
        I_Na = (m(i)^3)*gBar_Na*h(i)*(v(i)-e_Na);
        I_K = (n(i)^4)*gBar_K*(v(i)-e_K);
        I_L = gBar_L*(v(i)-e_L);
        I_ion = injectedCurrent-I_K-I_Na-I_L;
   
        v(i+1) = v(i)+timeStep*I_ion/c;
        m(i+1) = m(i)+timeStep*(a_m(i)*(1-m(i))-b_m(i)*m(i));
        n(i+1) = n(i)+timeStep*(a_n(i)*(1-n(i))-b_n(i)*n(i));
        h(i+1) = h(i)+timeStep*(a_h(i)*(1-h(i))-b_h(i)*h(i));
    
    end
    
    v = v-70;

    plot(t,v)
    hold on
    ylabel('Voltage (mV)');
    xlabel('Time (ms)');
    legend({'Voltage'});
    axis([0,100,-80,-60]);
    title('Membrane Potential of Steady-State Neuron');
    
    figure
    k = plot(t,(gBar_K*n.^4));
    hold on
    na = plot(t,(gBar_Na*(m.^3).*h),'r');
    legend([k,na],'Potassium','Sodium');
    ylabel('Conductance (mS/cm2)');
    xlabel('Time (ms)');
    title('Potassium and Sodium Channel Condutance at Steady-State');
end
