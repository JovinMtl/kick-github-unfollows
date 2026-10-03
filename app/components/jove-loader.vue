<template>
	<div class="main-container">
	    <div class="container-processing">
	        <div class="line line1"></div>
	        <div class="line not-first-line line2"></div>
	        <div class="line not-first-line line3"></div>
	        <div class="line not-first-line line4"></div>
	        <div class="line not-first-line line5"></div>
	    </div>
    </div>
</template>
<style scoped>
        .main-container{
            position: relative;
            display: inline-flex;
            transform: translateY(-4px);
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
            background-color: rgb(74, 229, 240);
            top : 50%;
            left : 20%;
            transform: translate(-50%, -50%);
            border-radius: 10px;
            transition-property: all;
            transition-duration: 0.3s;
            animation: agent1 1s ease-in-out infinite;
        }
        .line1:nth-child(3n){
            animation-delay: 5s;
        }
        .not-first-line{
            position: absolute;
            top : 50%;
            transform: translate(-50%, -50%);
            border-radius: 10px;
            transition-property: height;
            transition-duration: 0.3s;
        }
        .line2{
            background-color: rgb(136, 255, 0);
            left : 35%;
            animation: agent2 3s ease-in-out infinite;
        }
        .line3{
            background-color: rgb(74, 185, 236);
            left : 50%;
            animation: agent3 3s ease-in-out infinite;
        }
        .line4{
            background-color: rgb(74, 236, 182);
            left : 65%;
            animation: agent4 3s ease-in-out infinite;
        }
        .line5{
            background-color: rgb(77, 132, 233);
            left : 80%;
            animation: agent5 3s ease-in-out infinite;
        }
        @keyframes agent1 {
            0%, 100%{
                height: 50%;
            }
            50%{
                height: 75%;
            }
            75%{
                height: 90%;
            }
        }
        @keyframes agent2 {
            0%,20%, 100%{
                height: 50%;
            }
            25%,45%{
                height: 75%;
            }
            48%{
                height: 50%;
            }
            40%{
                height: 90%;
            }
            
        }
        @keyframes agent3 {
            0%,40%, 100%{
                height: 50%;
            }
            45%, 63%{
                height: 75%;
            }
            69%{
                height: 50%;
            }
            60%{
                height: 90%;
            }
        }
        @keyframes agent4 {
            0%,60%, 100%{
                height: 50%;
            }
            64%, 83%{
                height: 75%;
            }
            85%{
                height: 50%;
            }
            80%{
                height: 90%;
            }
        }
        @keyframes agent5 {
            0%,40%, 79%, 100%{
                height: 50%;
            }
            84%, 97%{
                height: 75%;
            }
            93%{
                height: 90%;
            }
        }
        
    </style>