<template>
  

    <div class="flex flex-1 items-center justify-center">
          <div class="rounded-lg sm:border-2 px-4 lg:px-20 py-16 lg:max-w-xl sm:max-w-md w-full text-center mt-48">

            <Form class="text-center" :Submitted="Submitted" @Submit="(data)=>{OnSubmit(data)}" @FailedSubmit="Submitted=false" :FormV="UserForgotPasswordFormV">
              <template #default={form,submiterror}>
                  <h1 class="font-bold tracking-wider text-3xl mb-8 w-full text-gray-600">
                  Forgot Password
                  </h1>
                  <div class="py-2 text-left">
                    <Input v-model="form.email" type="email" name="email" :errors="$FormValidation.GetFieldErrors(form.email,UserForgotPasswordFormV.shape.email)" Placeholder="Email" :submiterror="submiterror"/>
                  </div>
                </template>
                <template v-slot:button>
                  <div class="py-2">
                      <button @click="Submitted=true" type="submit" class="border-2 border-gray-100 focus:outline-none bg-purple-600 text-white font-bold tracking-wider block w-full p-2 rounded-lg focus:border-gray-700 hover:bg-purple-700">
                      Send Reset Password Link
                      </button>
                  </div>
                </template>
                  
    

              
              </Form>

          </div>
      </div>
  
  
    </template>
    
    <script setup>
  import { ToastrMessageType, ToastrPosition } from '~/plugins/notification';
import { UserForgotPasswordFormV } from '~/validation/UserForgotPasswordFormV';
  const Submitted = ref(false)
    
     
  
  const {$FormValidation,$http,$notify,$ErrorHandler,$inforoute} = useNuxtApp()
    
  
  function OnSubmit(data) {
    $http.Post("/auth/forgot-password",{body:data,onResponseError:(res)=>{$ErrorHandler.responseError(res)}}).then(()=>{
      $inforoute.route(true,"/auth/login","Password reset email sent We Are Routing You Main Login Page",ToastrMessageType.Success,ToastrPosition.TopCenter)
    },
    ()=>{
      Submitted.value=false
    })
  
  }

//  .then(()=>{
//    $notify("Email Sent Chech your EmailBox")
//  },()=>{Submitted.value = false})}
    </script>
  