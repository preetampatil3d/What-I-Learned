## Security

```
@Entity
@Table("UserInfo")
public class UserInfo{
  @GeneratedValue()
  @Id
  Integer id;

  String name;
  String username;
  String password;
  String roles; // comma separated
  // getter setter
}
@Repository
public interface UserInfoRepository extends JPARepository<UserInfo,Integer>{
  Optional<UserInfo> findByUsername(String username);
}

// Override all the method, we need this to convert UserInfo to UserDetails in UserDetailsService class
public class UserInfoToUserDetails implements UserDetails{ 

  private username;
  private password;
  private List<GrantedAuthority> authorities;

  public UserInfoToUserDetails(UserInfo useInfo){
    this.username = useInfo.getUsername();
    this.password = useInfo.getPassword();
    this.authorities = Arrays.stream(userInfo.getRoles().split(','))
                            .map(SimpleGrantedAuthority::new)
                            .collect(Collectors.toList());
  }
  @Override
  public Collection<? extends GrantedAuthority> getAuthorities(){
    return authorities;
  }
  @Override
  public String getUsername(){
    return username;
  }
  @Override
  public String getPassword(){
    return password;
  }
  @Override
  //isAccountNotLoacked: true , isCredentialsNonExpired true, isEnabled true
  
}

public class UserDetailsService implements UserDetailsService{
  @Autowire
  UserInfoRepository userRepo;
  @Override
  public loadUserByUsername(String username) throws UsernameNotFoundException{
    Optional<UserInfo> user = userRepo.findByUsername(username);

    return user.map(UserInfoToUserDetails::new)
        .orElseThrow(() -> new UserNotFoundException("Not Found"));
  }
}

// Security Config

@Configuration
public class SpringSecurityConfig{

@Bean
public UserDetailsService(PasswordEncoder encoder){
  return new UserInfoToUserDetails();
}
@Bean
public SecurityFilterChain securityFilter(HttpSecurity http){
  return http.csrf().disabled()
        .authorizeHttpRequests()
        .requestMatchers("/homePage").permitAll()
        .and()
        ...

}


}



```
