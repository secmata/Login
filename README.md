1 login/application/config/autoload.php

	$autoload['libraries'] = array('database');

2 login/application/config/database.php

	set up your databse.

3 login/application/config/autoload.php

	$autoload['libraries'] = array('session');

4 login/application/config/config.php

	sample site to get a encryption_key:
		http://randomkeygen.com/

	$config['encryption_key'] = '5iK0HbgVM7yE22N21MKX6042804lYVM5';

5 login/application/config/route.php

	$route['default_controller'] = "login";

6 login/application/views_site

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="utf-8">
	<title>CodeIgniter Login</title>
</head>
<body>

<div id="container">
	<h1>CodeIgniter Login</h1>
	<?php echo validation_errors();	?>
    <form method="POST" action="<?php echo base_url();?>">
    Username : <input type="text" name="username"> 
	Password : <input type="text" name="password">
    <input type="submit" value="Submit">
    </form>
</div>
</body>
</html>

7 login/application/controllers/site.php

<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

class Site extends CI_Controller {

	public function Site(){
		parent::__construct();
		$this->load->model('model_site');
	}
	
	public function index(){
		$this->_login_validation();
	}
	
	public function _login_validation(){
		if($this->session->userdata('login')){
			$this->_login();
		}else{
			$this->load->library('form_validation');
			$this->form_validation->set_rules('username', 'Username', 'required|callback__validation_one');
			$this->form_validation->set_rules('password', 'Password', 'required');
			if($this->form_validation->run()){
				$data = array('login' => 1);
				$this->session->set_userdata($data);
				
				$this->_login();
			}else{
				$this->load->view('view_login');
			}
		}
	}
	
	public function _validation_one(){
		$username = $this->input->post('username');
		$password = $this->input->post('password');
	
		if($this->model_site->log_in($username, $password)){
            return true;
        }else{
            $this->form_validation->set_message('validation_one', 'invalid user');
			return false;
        }
	}
	
	public function _login(){
		echo '<a href="' . base_url() . 'site/destroy">Logout</a><br>';
	}
	
	public function destroy(){
		$this->session->sess_destroy();
		redirect('');
	}
}

/* End of file welcome.php */
/* Location: ./application/controllers/site.php */

8 login/application/models/model_site

<?php
class Model_site extends CI_Model{
	public function log_in($email, $password){
		$query_str = 'SELECT * 
					  FROM user 
					  WHERE username = ? AND password = ?';
		
		$result = $this->db->query($query_str, array($email, $password));
		
        return $result->num_rows();
    }
}
