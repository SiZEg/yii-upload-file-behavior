# yii-upload-file-behavior
Yii upload file behavior

## 1. Installation

Put UploadFileBehavior.php to your project's extension dir
> protected\extensions\yii-upload-file-behavior\UploadFileBehavior.php

## 2. Examples

### Basic usage

Get your ActiveRecord model and set up it behavior:
```php
<?php
/**
 * This is the model class for table "{{profile}}"
 *
 * @property string $photo
 */
class Profile extends CActiveRecord {
    public function behaviors(){
        return array_merge( parent::behaviors(), array(
			'uploadFile' => array(
			    // Path alias to extension php file
				'class' => 'ext.yii-upload-file-behavior.UploadFileBehavior',
				// Model attribute, default is 'file'
				'attribute' => 'photo',
				// Path alias to your upload dir
				'pathAlias' => 'webroot.upload.profile',
			)
        ) );
    }
}
```

Set up your view form. It's very important to add 'enctype' => 'multipart/form-data' form attribute.
```php
<?php $form = $this->beginWidget( 'CActiveForm', array( 'htmlOptions' => array( 'enctype' => 'multipart/form-data' ) ) ); ?>
	<div class="form-group">
		<?php echo $form->labelEx( $model, 'photo' ); ?>
		<?php echo $form->fileField( $model, 'photo' ); ?>
		<?php echo $form->error( $model, 'photo' ); ?>
		<?php echo ( $model->photo ? '<div>Photo file name: '.$model->photo.'</div>' : ''; ?>
	</div>
<?php $this->endWidget(); ?>
```

Your controller will looks like as usual
```php
<?php
class ProfileController extends CController {
    public function actionCreate(){
        $model = new Profile;
        if ( isset( $_POST['Profile'] ) ) {
            $model->attributes = $_POST['Profile'];
            if ( $model->save() ) {
                $this->redirect( array('index') );
            }
        }
        
        $this->render( 'create', array(
            'model' => $model
        ) );
    }
}
```
