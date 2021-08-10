# 권한 설정과 표현식

  ~~~java
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication().withUser("user").password("{noop}qwer").roles("USER"); //1
        auth.inMemoryAuthentication().withUser("sys").password("{noop}qwer").roles("SYS"); //2
        auth.inMemoryAuthentication().withUser("admin").password("{noop}qwer").roles("ADMIN", "SYS", "USER"); //3
        // {noop} 으로 주게되면 암호화 설정 안함 ex) sha256 같은 설정을 주려면 별도의 설정 필요
        // 1번 설명 id user password qwer 인 유저에 USER 롤을 부여
        // 2번 설명 id sys password qwer 인 유저에 SYS 롤을 부여
        // 3번 설명 id admin password qwer 인 유저에 ADMIN, SYS, USER 롤을 부여 ( 복수 부여 )
    }

    // ctrl + o Method add
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests() //시큐리티 처리에 HttpServletRequest를 이용한다는 것을 의미합니다.
                .antMatchers("/user").hasRole("USER") // '/user' 경로와 회원 Role이 같은지 검증
                .antMatchers("/admin/pay").hasRole("ADMIN") // '/user' 경로와 회원 Role이 같은지 검증
                .antMatchers("/admin/**").access("hasRole('ADMIN') or hasRole('SYS')") // '/user' 경로와 회원 Role이 같은지 검증
                .anyRequest().authenticated(); // 어떠한 요청에도 인증을 받아야 하는 설정
        http
                .formLogin(); 
  ~~~

  