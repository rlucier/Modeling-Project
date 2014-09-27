Modeling-Project
====================

BMED 4784 Project
Problem 1 (Steady-State)

function steadyState()
    
    gBar_K = 36; %Maximum K channel conductance
    gBar_Na = 120; %Maximum Na channel conductance
    gBar_L= 0.3; %Maximum leakage channel conductance
    e_K = -12; %K Nernst potential
    e_Na = 115; %Na Nernst potential
    e_L = 10.6; %Leakage Nernst potential
    v = 0; %Baseline membrane voltage
    c = 1; %Membrane capacitance
    injectedCurrent = 0;
    
    %Rate constant equations
    a_m = 0.1*((25-v)/(exp((25-v)/10)-1)); 
    b_m = 4*exp(-v/18);
    a_n = 0.01*((10-v)/(exp((10-v)/10)-1));
    b_n = 0.125*exp(-v/80);
    a_h = 0.07*exp(-v/20);
    b_h = 1/(exp((30-v)/10)+1);

    m(1) = a_m/(a_m+b_m); %Probability K channels are activated
    n(1) = a_n/(a_n+b_n); %Probability Na channels are activated
    h(1) = a_h/(a_h+b_h); %Probability Na channels are inactivated

    tf = 100; %100ms 
    timeStep = 0.1; %Resolution
    t = 0:timeStep:tf; %Vector of time points
   
    %Iterate through the vector with time points
    for i = 1:numel(t)-1
   
        %Rate constants at each time step
        a_m(i) = 0.1*((25-v(i))/(exp((25-v(i))/10)-1));
        b_m(i) = 4*exp(-v(i)/18);
        a_n(i) = 0.01*((10-v(i))/(exp((10-v(i))/10)-1) );
        b_n(i) = 0.125*exp(-v(i)/80);
        a_h(i) = 0.07*exp(-v(i)/20);
        b_h(i) = 1/(exp((30-v(i))/10)+1);
   
        I_Na = (m(i)^3)*gBar_Na*h(i)*(v(i)-e_Na); %Current through Na channel
        I_K = (n(i)^4)*gBar_K*(v(i)-e_K); %Current through K channel
        I_L = gBar_L*(v(i)-e_L); %Current through leakage channel
        I_ion = injectedCurrent-I_K-I_Na-I_L; %Total current
   
        v(i+1) = v(i)+timeStep*I_ion/c; %Euler's Method for v
        m(i+1) = m(i)+timeStep*(a_m(i)*(1-m(i))-b_m(i)*m(i)); %Euler's method for m
        n(i+1) = n(i)+timeStep*(a_n(i)*(1-n(i))-b_n(i)*n(i)); %Euler's method for n
        h(i+1) = h(i)+timeStep*(a_h(i)*(1-h(i))-b_h(i)*h(i)); %Euler's method for h
    
    end
    
    v = v-70; %Baseline minus resting potential

    %Membrane potential figure
    figure
    plot(t,v)
    hold on
    ylabel('Voltage (mV)');
    xlabel('Time (ms)');
    legend({'Voltage'});
    axis([0,100,-80,-60]);
    title('Membrane Potential of Steady-State Neuron');
    
    gK = (n.^4)*gBar_K; %Conductances for K at certain time points
    gNa = (m.^3).*h*gBar_Na; %Conductances for Na at certain time points
    
    %Conductance figure
    figure
    k = plot(t,gK);
    hold on
    na = plot(t,gNa,'r');
    legend([k,na],'gK','gNa');
    ylabel('Conductance (mS/cm2)');
    xlabel('Time (ms)');
    title('Potassium and Sodium Channel Condutances at Steady-State');
end

Problem 2 (Step Pulse)

function stepPulse()
    
    gBar_K = 36; %Maximum K channel conductance
    gBar_Na = 120; %Maximum Na channel conductance
    gBar_L= 0.3; %Maximum leakage channel conductance
    e_K = -12; %K Nernst potential
    e_Na = 115; %Na Nernst potential
    e_L = 10.6; %Leakage Nernst potential
    v = 0; %Baseline membrane voltage
    c = 1; %Membrane capacitance
    injectedCurrent = 5; %Step pulse of 5uA/cm2
    
    %Rate constant equations
    a_m = 0.1*((25-v)/(exp((25-v)/10)-1));
    b_m = 4*exp(-v/18);
    a_n = 0.01*((10-v)/(exp((10-v)/10)-1));
    b_n = 0.125*exp(-v/80);
    a_h = 0.07*exp(-v/20);
    b_h = 1/(exp((30-v)/10)+1);

    m(1) = a_m/(a_m+b_m); %Probability K channels are activated
    n(1) = a_n/(a_n+b_n); %Probability Na channels are activated
    h(1) = a_h/(a_h+b_h); %Probability Na channels are inactivated

    tf = 100; %100ms 
    timeStep = 0.01; %Resolution
    t = 0:timeStep:tf; %Vector of time points
    
    I(1:500) = injectedCurrent; %Create an injected current vector with the first 0.5ms with the injected current
    I(501:numel(t)) = 0; %Remainder of injected current vector is 0
   
    %Iterate through the vector with time points
    for i = 1:numel(t)-1
   
        %Rate constants at each time step
        a_m(i) = 0.1*((25-v(i))/(exp((25-v(i))/10)-1));
        b_m(i) = 4*exp(-v(i)/18);
        a_n(i) = 0.01*((10-v(i))/(exp((10-v(i))/10)-1) );
        b_n(i) = 0.125*exp(-v(i)/80);
        a_h(i) = 0.07*exp(-v(i)/20);
        b_h(i) = 1/(exp((30-v(i))/10)+1);
   
        I_Na = (m(i)^3)*gBar_Na*h(i)*(v(i)-e_Na); %Current through Na channel
        I_K = (n(i)^4)*gBar_K*(v(i)-e_K); %Current through K channel
        I_L = gBar_L*(v(i)-e_L); %Current through leakage channel
        I_ion = I(i)-I_K-I_Na-I_L; %Total current
   
        v(i+1) = v(i)+timeStep*I_ion/c; %Euler's Method for v
        m(i+1) = m(i)+timeStep*(a_m(i)*(1-m(i))-b_m(i)*m(i)); %Euler's method for m
        n(i+1) = n(i)+timeStep*(a_n(i)*(1-n(i))-b_n(i)*n(i)); %Euler's method for n
        h(i+1) = h(i)+timeStep*(a_h(i)*(1-h(i))-b_h(i)*h(i)); %Euler's method for h
    
    end
    
    v = v-70; %Baseline minus resting potential

    %Membrane potential figure
    figure
    plot(t,v)
    hold on
    ylabel('Voltage (mV)');
    xlabel('Time (ms)');
    legend({'Voltage'});
    title('Membrane Potential of Neuron during Step Pulse of 5uA/cm2');
    
    gK = (n.^4)*gBar_K; %Conductances for K at certain time points
    gNa = (m.^3).*h*gBar_Na; %Conductances for Na at certain time points
    
    %Conductance figure
    figure
    k = plot(t,gK);
    hold on
    na = plot(t,gNa,'r');
    legend([k,na],'gK','gNa');
    ylabel('Conductance (mS/cm2)');
    xlabel('Time (ms)');
    title('Potassium and Sodium Channel Condutances during Step Pulse of 5uA/cm2');
end

Problem 3 (Constant Current)

function constantCurrent()
    
    gBar_K = 36; %Maximum K channel conductance
    gBar_Na = 120; %Maximum Na channel conductance
    gBar_L= 0.3; %Maximum leakage channel conductance
    e_K = -12; %K Nernst potential
    e_Na = 115; %Na Nernst potential
    e_L = 10.6; %Leakage Nernst potential
    v = 0; %Baseline membrane voltage
    c = 1; %Membrane capacitance
    injectedCurrent = 5; %Constant current of 5uA/cm2
    
    %Rate constant equations
    a_m = 0.1*((25-v)/(exp((25-v)/10)-1)); 
    b_m = 4*exp(-v/18);
    a_n = 0.01*((10-v)/(exp((10-v)/10)-1));
    b_n = 0.125*exp(-v/80);
    a_h = 0.07*exp(-v/20);
    b_h = 1/(exp((30-v)/10)+1);

    m(1) = a_m/(a_m+b_m); %Probability K channels are activated
    n(1) = a_n/(a_n+b_n); %Probability Na channels are activated
    h(1) = a_h/(a_h+b_h); %Probability Na channels are inactivated

    tf = 100; %100ms 
    timeStep = 0.01; %Resolution
    t = 0:timeStep:tf; %Vector of time points
   
    %Iterate through the vector with time points
    for i = 1:numel(t)-1
   
        %Rate constants at each time step
        a_m(i) = 0.1*((25-v(i))/(exp((25-v(i))/10)-1));
        b_m(i) = 4*exp(-v(i)/18);
        a_n(i) = 0.01*((10-v(i))/(exp((10-v(i))/10)-1) );
        b_n(i) = 0.125*exp(-v(i)/80);
        a_h(i) = 0.07*exp(-v(i)/20);
        b_h(i) = 1/(exp((30-v(i))/10)+1);
   
        I_Na = (m(i)^3)*gBar_Na*h(i)*(v(i)-e_Na); %Current through Na channel
        I_K = (n(i)^4)*gBar_K*(v(i)-e_K); %Current through K channel
        I_L = gBar_L*(v(i)-e_L); %Current through leakage channel
        I_ion = injectedCurrent-I_K-I_Na-I_L; %Total current
   
        v(i+1) = v(i)+timeStep*I_ion/c; %Euler's Method for v
        m(i+1) = m(i)+timeStep*(a_m(i)*(1-m(i))-b_m(i)*m(i)); %Euler's method for m
        n(i+1) = n(i)+timeStep*(a_n(i)*(1-n(i))-b_n(i)*n(i)); %Euler's method for n
        h(i+1) = h(i)+timeStep*(a_h(i)*(1-h(i))-b_h(i)*h(i)); %Euler's method for h
    
    end
    
    v = v-70; %Baseline minus resting potential

    %Membrane potential figure
    figure
    plot(t,v)
    hold on
    ylabel('Voltage (mV)');
    xlabel('Time (ms)');
    legend({'Voltage'});
    title('Membrane Potential of Neuron during Constant Current of 5uA/cm2');
   
    gK = (n.^4)*gBar_K; %Conductances for K at certain time points
    gNa = (m.^3).*h*gBar_Na; %Conductances for Na at certain time points
    
    %Conductance figure
    figure
    k = plot(t,gK);
    hold on
    na = plot(t,gNa,'r');
    legend([k,na],'gK','gNa');
    ylabel('Conductance (mS/cm2)');
    xlabel('Time (ms)');
    title('Potassium and Sodium Channel Condutances during Constant Current of 5uA/cm2');
end
