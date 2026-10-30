# K6 lab results 
  
## Part 0 
- Step 0.1, http_req_duration: avg=317.13ms min=317.13ms med=317.13ms max=317.13ms p(90)=317.13ms p(95)=317.13ms
- Step 0.2, requests, rate and p(95), and why the rate stays below 5: 70, 3.4077 , 461.2 ms because it have sleep that make it take 1.4 second to complete one cycle.
- Step 0.6, exit code of the normal run and of the run with P95=300: no it cross to 457.82 ms
- Step 0.7, rules that passed, p(95), p(99), exit code after your change: all rule that pass and change 1000 to 100 to not pass
- Step 0.8, what was wrong, and how the checks showed it: The wrong is the pepproni and knife not be a capital latter it need to be Pepperoni and Knife and the check show 
 no pepperoni
      ↳  90% — ✓ 68 / ✗ 7
✗ no knife needed
      ↳  60% — ✓ 45 / ✗ 30
  
## Part A results 
1.
| Step | Test type    | Users | Req/s | p(95) | Max    | Failed | Checks | 
| ---- | ------------ | ----- | ----- | ----- | ---    | ------ | ------ | 
| A1   | Smoke        | 1     |0.472  |412.66 |432.44ms|   0    |   20   | 
| A2   | Average load | 3     |1.269  |418.77 |652.93ms|   0     |   308     | 
| A3   | Stress       | 5     | 2.138 | 417.84|1.41s   |    0    |    528    | 
| A4   | Spike        | 5     |1.010  |431.05v|81.08ms |    0    |  192      | 
| A5   | Soak         | 2     | 0.965  |418.85  |537.93ms |   0     |    852    | 
  
## Part B 

2. the script contained a `sleep(1)` statement. This pause caused each iteration to take roughly 1.46 seconds. With 5 virtual users each taking over a second to complete one cycle
3. Grow with a 1.269 to 2.138 that mean the stress have to run harder more of the average that cause the stress make more response
4. 500 ms
5. the response time and error rate did **not** get worse over time (p95 remained stable at 418.85ms with 0 failures). A real soak test running for several hours is designed to reveal resource exhaustion issues, most notably **memory leaks** or unclosed database connections that eventually crash the system.
6. An acceptable average response time could mask the fact that 10% of users experienced terrible, 5-second load times. Using p(95) guarantees that 95% of all requests are meeting the performance requirement, ensuring a consistent experience for the vast majority of users.
7. Traffic over the public internet deals with variable routing hops, latency, and bandwidth constraints, whereas a lab uses a fast, controlled local network.
8. Running stress or spike tests against a site you do not own is indistinguishable from a Distributed Denial of Service (DDoS) attack. It degrades the experience for real users, costs the owner money in auto-scaling cloud fees, and is illegal.
9.**Vague:** "The mobile banking app should log me in quickly."
* **Testable:** "95% of login requests must complete within 800ms, and the failure rate must be less than 1%."
* **k6 Thresholds:** 
  ```javascript
  thresholds: {
    http_req_duration: ['p(95)<800'],
    http_req_failed: ['rate<0.01'],
  }