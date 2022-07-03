# OTP_authentication
# login_controller_Send_otp
       public function sendOtp(Request $request){
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
# Login with otp_controller
    public function loginWithOtp(Request $request){
        Log::info($request);

        $user  = User::where([['mobile','=',request('mobile')],['otp','=',request('otp')]])->first();

        if( $user){
            Auth::login($user, true);
            User::where('mobile','=',$request->mobile)->update(['otp' => null]);
            return redirect('/checkout');
        }
        else{
            return Redirect::back();
        }
    }
# Send otp API_LOGIN
 // Mobile OTP Start
       public function sendOtpApi(Request $request){
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
    // End Mobile OTP
    
 # login_otp_api_login
    
    // Mobile Login OTP API Start
       public function loginWithOtpApi(Request $request){
        Log::info($request);
        $user  = User::where([['mobile','=',request('mobile')],['otp','=',request('otp')]])->first();
        if( $user){
            Auth::login($user, true);
            User::where('mobile','=',$request->mobile)->update(['otp' => null]);
            $token = $user->createToken('myapptoken')->plainTextToken;
            $response = [
                'user' => $user,
                'token' => $token
            ];
            return response($response,201);
        }
        else{
            return response()->json(['message'=>'login failed'], 401);
        }
    }
    // Mobile Login OTP API End
