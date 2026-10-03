<template>
	<div style="position: relative;display: inline-flex
;
    align-items: flex-start;
    flex-wrap: wrap;
    flex-direction: column;
    justify-content: space-between;transform: translateY(-4px);">
	    <div class="container-processing">
	        <div class="line line1"></div>
	        <div class="line line2"></div>
	        <div class="line line3"></div>
	        <div class="line line4"></div>
	        <div class="line line5"></div>
	    </div>
    </div>
</template>
<style scoped>
        *{
            margin: 0;
            padding: 0;
        }
        body{
            position: relative;
            width: 100vw;
            height: 100vh;
        }
        .container-processing{
            position: absolute;
            height: 30px;
            width: 28px;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        .line{
            height: 50%;
            width: 3%; 
        }
        .line1{
            position: relative;
            background-color: red;

        background-color: rgb(74, 229, 240);
            top : 50%;
            left : 20%;
            transform: translate(-50%, -50%);
            border-radius: 10px;
            transition-property: all;
            transition-duration: 0.3s;
            animation-name: agent1;
            animation-duration: 1s;
            animation-iteration-count: infinite;
            animation-timing-function: ease-in-out;
        }
        .line1:nth-child(3n){
            animation-delay: 5s;
        }
        .line2{
            position: absolute;
            background-color: rgb(0, 132, 255);
            left : 35%;
            top : 50%;
            transform: translate(-50%, -50%);
            border-radius: 10px;
            transition-property: all;
            transition-duration: 0.3s;
            animation-name: agent2;
            animation-duration: 3s;
            animation-iteration-count: infinite;
        }
        .line3{
            position: absolute;
            background-color: rgb(74, 185, 236);
            left : 50%;
            top : 50%;
            transform: translate(-50%, -50%);
            border-radius: 10px;
            transition-property: all;
            transition-duration: 0.3s;
            animation-name: agent3;
            animation-duration: 3s;
            animation-iteration-count: infinite;
        }
        .line4{
            position: absolute;
            background-color: rgb(74, 236, 182);
            left : 65%;
            top : 50%;
            transform: translate(-50%, -50%);
            border-radius: 10px;
            transition-property: all;
            transition-duration: 0.3s;
            animation-name: agent4;
            animation-duration: 3s;
            animation-iteration-count: infinite;
        }
        .line5{
            position: absolute;
            background-color: rgb(8, 165, 139);
            left : 80%;
            top : 50%;
            transform: translate(-50%, -50%);
            border-radius: 10px;
            transition-property: all;
            transition-duration: 0.3s;
            animation-name: agent5;
            animation-duration: 3s;
            animation-iteration-count: infinite;
        }
        @keyframes agent1 {
            0%, 100%{
                height: 50%;
            }
            50%{
                height: 75%;
                box-shadow: 0 0 45px red;
            }
            75%{
                height: 90%;
                box-shadow: 0 0 35px red;
            }
        }
        @keyframes agent2 {
            0%,20%, 100%{
                height: 50%;
            }
            25%,45%{
                height: 75%;
                box-shadow: 0 0 35px rgb(217, 255, 0);
            }
            48%{
                height: 50%;
            }
            40%{
                height: 90%;
                box-shadow: 0 0 35px rgb(217, 255, 0);
            }
            
        }
        @keyframes agent3 {
            0%,40%, 100%{
                height: 50%;
            }
            45%, 63%{
                height: 75%;
                box-shadow: 0 0 45px rgb(0, 255, 34);
            }
            69%{
                height: 50%;
            }
            60%{
                height: 90%;
                box-shadow: 0 0 35px rgb(0, 255, 34);
            }
        }
        @keyframes agent4 {
            0%,60%, 100%{
                height: 50%;
            }
            64%, 83%{
                height: 75%;
                box-shadow: 0 0 45px blueviolet;
            }
            85%{
                height: 50%;
            }
            80%{
                height: 90%;
                box-shadow: 0 0 35px blueviolet;
            }
        }
        @keyframes agent5 {
            0%,40%, 79%, 100%{
                height: 50%;
            }
            84%, 97%{
                height: 75%;
                box-shadow: 0 0 45px rgb(8, 165, 139);
            }
            93%{
                height: 90%;
                box-shadow: 0 0 35px rgb(8, 165, 139);
            }
        }
        
    </style>