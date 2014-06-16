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
