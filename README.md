#!/bin/bash

while [[ $# > 0 ]]
do
    key="$1"
    case $key in
        -b|--Bucket_Manager)
            nextArg="$2"
            while ! [[ "$nextArg" =~ -.* ]] && [[ $# > 1 ]]; do
                case $nextArg in
                    create)
                        echo 'Bucket Name :'
                        read Bucket_Name
                        aws s3 mb s3://$Bucket_Name
                    ;;

                    delete)
                        echo 'BucketName :'
                        read Bucket_Name
                        aws s3 rb s3://$Bucket_Name --force
                    ;;

		    permissionread)
                        echo 'Username : '
                        read UserName
                        aws iam attach-user-policy --user-name $UserName --policy-arn arn:aws:iam::788554450215:policy/kawanlabs-access
                    ;;

		    permissionwrite)
                        echo 'Username : '
                        read UserName
                        aws iam attach-user-policy --user-name $UserName --policy-arn arn:aws:iam::788554450215:policy/kawanlabs-Fullaccess
                    ;;

		    delete-user-readonly)
                        echo 'Username :'
                        read UserName
                        aws iam detach-user-policy --user-name $UserName --policy-arn arn:aws:iam::788554450215:policy/kawanlabs-access
                     ;;

                    delete-use-rwrite)
                        echo 'Username :'
                        read UserName
                        aws iam detach-user-policy --user-name $UserName --policy-arn arn:aws:iam::788554450215:policy/kawanlabs-Fullaccess
                     ;;

                    *)
                        echo "$key $nextArg Found"
                    ;;
                esac
                if ! [[ "$2" =~ -.* ]]; then
                    shift
                    nextArg="$2"
                else
                    shift
                    break
                fi
            done
        ;;
        
		*)
            echo "Unknown flag $key"
        ;;
    esac
    shift
done
