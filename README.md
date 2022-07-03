# OTP_authentication
# login_controller
*** public function sendOtp(Request $request){
       $otp = rand(1000,9999);
        Log::info("otp = ".$otp);
        // $user = User::where('mobile','=',$request->mobile)->update(['otp' => $otp]);
        $input = $request->all();
        $input['otp'] = $otp;

        $user = DB::table('users')->where('mobile', $request->mobile)->first();
        if($user == true){
          $updateUserOTP = User::where('mobile','=',$request->mobile)->update(['otp' => $otp]);
        }
        else{
            $user = new User();
            $supplier_code = User::count();
            if($supplier_code < 10){
                $user->customer_id = '#000000'. $supplier_code;
            }elseif($supplier_code <=100){
                $user->customer_id = '#00000'. $supplier_code;
            }
            elseif($supplier_code <=1000){
                $user->customer_id = '#0000'. $supplier_code;
            } elseif($supplier_code <=10000){
                $user->customer_id = '#000'. $supplier_code;
            } elseif($supplier_code <=100000){
                $user->customer_id = '#00'. $supplier_code;
            }
            else{
                $users->customer_id = '#0'. $supplier_code;
            }
            $user->mobile = $request->mobile;
            $user->otp = $otp;

            $user->save();

        }

        $url = "http://66.45.237.70/api.php";
        $number=$request->mobile;
        $text="Your Bpp Shops OTP Code is ".$otp;
        $data= array(
        'username'=>"ShahidMahmum",
        'password'=>"Shahid26@.",
        'number'=>"$number",
        'message'=>"$text"
        );

        $ch = curl_init(); // Initialize cURL
        curl_setopt($ch, CURLOPT_URL,$url);
        curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($data));
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        $smsresult = curl_exec($ch);
        $p = explode("|",$smsresult);
        $sendstatus = $p[0];


    }
*** 
