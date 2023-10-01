# TC Tech Acamdemy week 4
#### Preview
---
Spring Security

# 1. 스프링 시큐리티란?
스프링 기반 애플리케이션의 인증과 권한을 담당하는 스프링의 하위 프레임워크
- 인증(Authenticate)은 사용자의 신원을 입증하는 과정으로, 로그인을 의미한다.
- 권한(Authorize)은 인증된 사용자가 어떤 것을 할 수 있는지를 의미한다.
이름에 걸맞게 보안 관련 옵션을 다수 제공하며 복잡한 로직 없이 어노테이션으로 설정 가능
여러 보안 위협을 방어해주고 요청 헤더도 보안 처리 해줌. 기본적으로 세션 기반 인증 제공

# 2. 스프링 시큐리티 동작 방식
스프링 시큐리티는 필터 기반으로 동작한다.
필터의 종류는 매우 다양하며, 각 필터에서 인증, 인가와 관련된 작업을 처리한다.

![image](https://github.com/leemhmh/myblog/assets/91586116/94b1b500-bccf-47ce-ac80-fda4912902fc)

SecurityContextPersistenceFilter부터 시작해서 아래로 내려가며 FilterSecurityIntercepter까지 순서대로 필터를 거침.
어피치 스티커가 붙어 있는 Intercepter는 아이디와 패스워드가 넘어오면 인증 요청을 위임하는 인증 관리자 역할을 함
프로도 스티커가 붙어있는 Intercepter는 권한 부여 처리를 위임해 접근 제어 결정을 쉽게 하는 접근 결정 관리자 역할을 함

![image](https://github.com/leemhmh/myblog/assets/91586116/8b0ce395-d85b-41b9-9747-7af4b05cbdc2)
![image](https://github.com/leemhmh/myblog/assets/91586116/94c6f6d0-61ba-47fa-aded-0009b007c83b)



구현 예시(출처: https://adjh54.tistory.com/92)
---
Spring Security를 이용하기 위한 객체로 사용
UserDetails의 실제 구현체
```
package com.adjh.multiflexapi.model;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.experimental.Delegate;
import lombok.extern.slf4j.Slf4j;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java.util.Collection;

@Slf4j
@Getter
@AllArgsConstructor
public class UserDetailsDto implements UserDetails {

    @Delegate
    private UserDto userDto;
    private Collection<? extends GrantedAuthority> authorities;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return authorities;
    }

    @Override
    public String getPassword() {
        return userDto.getUserPw();
    }

    @Override
    public String getUsername() {
        return userDto.getUserNm();
    }

    @Override
    public boolean isAccountNonExpired() {
        return false;
    }

    @Override
    public boolean isAccountNonLocked() {
        return false;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return false;
    }

    @Override
    public boolean isEnabled() {
        return false;
    }
}
```
Spring Security를 이용하기 위한 구현체로 사용
UserDetailsService의 구현체로 사용
```
package com.adjh.multiflexapi.service.impl;


import java.util.Collections;

import com.adjh.multiflexapi.model.UserDetailsDto;
import com.adjh.multiflexapi.model.UserDto;
import com.adjh.multiflexapi.service.UserService;
import org.springframework.security.authentication.AuthenticationServiceException;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.stereotype.Service;

@Service
public class UserDetailsServiceImpl implements UserDetailsService {

    private final UserService userService;

    public UserDetailsServiceImpl(UserService us) {
        this.userService = us;
    }

    @Override
    public UserDetails loadUserByUsername(String userId) {
        UserDto userDto = UserDto
                .builder()
                .userId(userId)
                .build();

        // 사용자 정보가 존재하지 않는 경우
        if (userId == null || userId.equals("")) {
            return userService.login(userDto)
                    .map(u -> new UserDetailsDto(u, Collections.singleton(new SimpleGrantedAuthority(u.getUserId()))))
                    .orElseThrow(() -> new AuthenticationServiceException(userId));
        }
        // 비밀번호가 맞지 않는 경우
        else {
            return userService.login(userDto)
                    .map(u -> new UserDetailsDto(u, Collections.singleton(new SimpleGrantedAuthority(u.getUserId()))))
                    .orElseThrow(() -> new BadCredentialsException(userId));
        }
    }

}
```
데이터 베이스 내에 존재하는 사용자인지에 대한 확인을 위한 DB 데이터 조회 로직을 추가
1. 비즈니스 로직 중 '로그인'에 사용이 되는 Model 객체를 생성
```
package com.adjh.multiflexapi.model;

import lombok.AccessLevel;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class UserDto {

    // 사용자 시퀀스
    private int userSq;

    // 사용자 아이디
    private String userId;

    // 사용자 패스워드
    private String userPw;

    // 사용자 이름
    private String userNm;

    // 사용자 상태
    private String userSt;

    @Builder
    UserDto(int userSq, String userId, String userPw, String userNm, String userSt) {
        this.userSq = userSq;
        this.userId = userId;
        this.userPw = userPw;
        this.userNm = userNm;
        this.userSt = userSt;
    }

}
```
2. 로그인 정보를 DB에서 확인하기 위한 Service interface 구축
```
package com.adjh.multiflexapi.service;

import com.adjh.multiflexapi.model.UserDto;

import java.util.Optional;

public interface UserService {
    Optional<UserDto> login(UserDto userVo);
}
```
3. 로그인 정보를 DB에서 확인하기 위한 Service interface의 구현체
```
ackage com.adjh.multiflexapi.service.impl;

import com.adjh.multiflexapi.mapper.CodeMapper;
import com.adjh.multiflexapi.mapper.UserMapper;
import com.adjh.multiflexapi.model.UserDto;
import com.adjh.multiflexapi.service.UserService;
import lombok.extern.slf4j.Slf4j;
import org.apache.ibatis.session.SqlSession;
import org.springframework.stereotype.Service;

import java.util.Optional;

@Service
@Slf4j
public class UserServiceImpl implements UserService {

    private final SqlSession sqlSession;

    public UserServiceImpl(SqlSession ss) {
        this.sqlSession = ss;
    }

    /**
     * 로그인 구현체
     *
     * @param userDto UserDto
     * @return Optional<UserDto>
     */
    @Override
    public Optional<UserDto> login(UserDto userDto) {
        UserMapper um = sqlSession.getMapper(UserMapper.class);
        return um.login(userDto);
    }
}
```
4. 로그인 정보를 DB에서 확인하기 위한 SQL문과의 매핑을 수행하는 인터페이스 구축
```
package com.adjh.multiflexapi.mapper;

import com.adjh.multiflexapi.model.UserDto;
import org.springframework.stereotype.Repository;

import java.util.Optional;

@Repository
public interface UserMapper {

    Optional<UserDto> login(UserDto userDto);

}
```
5. 데이터 베이스 내에서 사용자 아이디를 기반으로 사용자를 조회
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.adjh.multiflexapi.mapper.UserMapper">

    <!--로그인-->
    <select id="login" resultType="userDto">
        SELECT t1.*
        FROM multiflex_scma.tb_user t1
        WHERE user_id = #{userId}
    </select>
</mapper>
```
Spring Security 환경설정
1. Spring Security의 환경설정을 구성하기 위한 클래스 구현
    - 웹 서비스가 로드될 때 Spring Container에 의해 관리가 되는 클래스이며 사용자에 대한 ‘인증’과 ‘인가’에 대한 구성을 Bean 메서드로 주입
2. 커스텀을 수행한 '인증' 필터로 접근 URL, 데이터 전달 방식(form) 등 인증 과정 및 인증 후 처리에 대한 설정을 구성하는 메서드 구현
   - 최종 인증이 완료되면 사용자 아이디와 비밀번호 기반으로 토큰을 발급합니다.
3. 전달받은 사용자의 아이디와 비밀번호를 기반으로 비즈니스 로직을 처리하여 사용자의 ‘인증’에 대해서 검증을 수행하는 클래스 구현
4. 사용자의 ‘인증’에 대해 성공하였을 경우 수행되는 Handler로 성공에 대한 사용자에게 반환 값을 구성하여 전달하는 클래스 구현
5. 사용자의 ‘인증’에 대해 실패하였을 경우 수행되는 Handler로 실패에 대한 사용자에게 반환값을 구성하여 전달하는 클래스 구현


## 위와 같은 구현들을 통해 로그인 기능을 구축할 수 있다.

#회원 가입 구현(참조: https://devhan.tistory.com/310)
