### Hello World!
```java
public class RaIN{
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
```

- 弹窗对话框

layout.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:paddingTop="13dp"
    android:background="@drawable/shape_download_video">

    <TextView
        android:id="@+id/tv_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="下载视频"
        android:textColor="@color/black"
        android:textSize="18sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.0" />

    <TextView
        android:id="@+id/tv_pay_tip"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="5dp"
        android:text="支付方式"
        android:textColor="@color/black"
        android:textSize="12sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/tv_title"
        app:layout_constraintVertical_bias="0.0" />

    <View
        android:id="@+id/v_divider"
        android:layout_width="match_parent"
        android:layout_height="1.5dp"
        android:layout_marginTop="12dp"
        android:background="@color/gray5"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/tv_pay_tip" />

    <TextView
        android:id="@+id/tv_pay_wx"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:padding="10dp"
        android:text="微信"
        android:textColor="@color/dodgerblue"
        android:textSize="16sp"
        app:layout_constraintEnd_toStartOf="@id/tv_pay_ali"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/v_divider" />

    <TextView
        android:id="@+id/tv_pay_ali"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:padding="10dp"
        android:text="支付宝"
        android:textColor="@color/dodgerblue"
        android:textSize="16sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toEndOf="@id/tv_pay_wx"
        app:layout_constraintTop_toBottomOf="@id/v_divider" />

</android.support.constraint.ConstraintLayout>
```

DownloadVideoTipDialogFragment.java

```java
import android.app.Dialog;
import android.app.DialogFragment;
import android.graphics.Color;
import android.graphics.drawable.ColorDrawable;
import android.os.Bundle;
import android.support.annotation.Nullable;
import android.util.DisplayMetrics;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

public class DownloadVideoTipDialogFragment extends DialogFragment {

    private onPayClickListener onPayClickListener;

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, Bundle savedInstanceState) {
        getDialog().getWindow().setBackgroundDrawable(new ColorDrawable(Color.TRANSPARENT));
        View view = inflater.inflate(R.layout.dialog_download_video_tip, container, false);
        view.findViewById(R.id.tv_pay_wx).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (onPayClickListener != null) {
                    onPayClickListener.onWxPay();
                    dismiss();
                }
            }
        });
        view.findViewById(R.id.tv_pay_ali).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (onPayClickListener != null) {
                    onPayClickListener.onAliPay();
                    dismiss();
                }
            }
        });
        return view;
    }

    @Override
    public void onStart() {
        super.onStart();
        Dialog dialog = getDialog();
        if (dialog != null) {
            DisplayMetrics displayMetrics = new DisplayMetrics();
            getActivity().getWindowManager().getDefaultDisplay().getMetrics(displayMetrics);
            dialog.getWindow().setLayout((int) (displayMetrics.widthPixels * 0.7), ViewGroup.LayoutParams.WRAP_CONTENT);
        }
    }

    public interface onPayClickListener{
        void onWxPay();
        void onAliPay();
    }

    public void setOnPayClickListener(DownloadVideoTipDialogFragment.onPayClickListener onPayClickListener) {
        this.onPayClickListener = onPayClickListener;
    }
}

```

