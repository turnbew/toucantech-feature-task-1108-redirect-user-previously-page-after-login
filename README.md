FOR PRIVACY AND CODE PROTECTING REASONS THIS IS A SIMPLIFIED VERSION OF CHANGES AND NEW FEATURES

TASK DATE: 22.11.2017 - Finished: 22.11.2017

TASK LEVEL: MEDIUM - light

TASK SHORT DESCRIPTION: 1108 (Redirect users to the previously viewed page after logging in)
	
GITHUB REPOSITORY CODE: feature/task-1108-redirect-user-previously-page-after-login

CHANGES
 
	IN FILES: 
	
		MY_Controller.php
		
			ADDED CODE inside function __construct
			
				//Preserve current page's url - after login this site will be loaded automatically
				//The system gets a lot of request (AJAX and other circulated requests totally unnecessarily), so need to filter the requests
				$current_url = current_url();
				$pattern = '~.+\.[a-zA-Z]{0,3}$~';
				$needles = array('users/login', 'files/', 'uploads/', 'homepage', 'error/', 'no-permission');
				preg_match($pattern, $current_url, $matches);
				if (
						.................
					)	
				{
					$this->session->set_userdata('page_url',  $current_url);
				}

				
		admin.php
		
			CHANGED CODE: 
			
				//Inside function _check_login
				
					FROM: if ($this->ion_auth->login($email, $this->input->post('password'), (bool)$this->input->post('remember'))) 
					
					TO: if ($this->ion_auth->login($email, $this->input->post('password'), (bool)$this->input->post('remember'), true))
					
					
		users.php
		
			CHANGED CODE:
		
				//Inside function _check_login
				
					FROM: if ($this->ion_auth->login($email, $this->input->post('password'), $remember))
					
					TO: if ($this->ion_auth->login($email, $this->input->post('password'), $remember, true))
					
				//Inside function register
				
					FROM: $this->ion_auth->login($this->input->post('email'), $this->input->post('password'));
					
					TO: $this->ion_auth->login($this->input->post('email'), $this->input->post('password'), true);
					
		
		ion_auth_model.php
		
			ADDED CODE inside function login
			
				if($this->session->userdata('page_url') and !$just_check) 
				{
					redirect($this->session->userdata('page_url'));
					exit;
				}			
				
				
		url_helper.php
			
			ADDED CODE inside function current_url
			
				return $_SERVER['QUERY_STRING'] ? $url . '?'  .$_SERVER['QUERY_STRING'] : $url;
		
			
		MY_text_helper.php
			
			ADDED CODE: 
			
				if ( ! function_exists('strpos_arr'))
				{
					/* 
					 * strpos that takes an array of values to match against a string
					 * note the stupid argument order (to match strpos)
					 */
					function strpos_arr($haystack, $needles) 
					{
						...................
						return false;
					}

				}
		
		
